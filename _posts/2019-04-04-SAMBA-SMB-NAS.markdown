---
layout: post
title:  "Enabling NAS with SAMBA"
date:   2019-04-04 12:00:40 -0700
categories: Networking
---

I want to create a Network-attached storage (NAS) system for my home network because that'd be cool. I don't need NAS  because file sharing across platforms is an issue that [Dropbox](www.dropbox.com) seems to have completely solved. I pay for the Dropbox Plus Plan, so I can have up to 1 TB of files automatically updated across my macOS, Windows, & Linux systems ... it even updates my files to my Android phone! I don't have an iPhone, but I imagine the service would work just as well over there.  

Actually, I'm not even currently capable of building a system as good as Dropbox because I don't yet know how to create a VPN that my off-site devices could get data from ... so, to be really clear, this NAS project is just for fun!

As has become the norm, I'll be building my NAS with a Raspberry Pi; it's got 32 GB of storage, so it can already kinda act as a NAS, but I'm adding a 1 TB USB drive to give is the sort of storage that I'd like. I've seen some tutorials that create a RAID-like system with multiple 1 TB drives, but I don't yet have the equipment to make a system like that.

If you're interested in following along, I'm using the [How-To Geek guide located here.](https://www.howtogeek.com/139433/how-to-turn-a-raspberry-pi-into-a-low-power-network-storage-device/) I could use PuTTY in Windows to connect, but I prefer SSH, so I'll log on to my Pi from Ubuntu like this:

```console
pi@raspberrypi:~ $ ssh pi@192.168.1.3
The authenticity of host '192.168.1.3 (192.168.1.3)' can't be established.
ECDSA key fingerprint is SHA256:YNNhELJ5x8AWL48H5mYA0ueC+5nElZkmYKy6wNVeFHE.
Are you sure you want to continue connecting (yes/no)? y
Please type 'yes' or 'no': yes
Warning: Permanently added '192.168.1.3' (ECDSA) to the list of known hosts.
pi@192.168.1.3's password:
Linux raspberrypi 4.14.98-v7+ #1200 SMP Tue Feb 12 20:27:48 GMT 2019 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Apr  5 10:48:07 2019 from 192.168.1.2
```

The 1 TB drive that I'll be using as a NAS is formatted as NTFS, so Linux doesn't innately have support for it. I could reformat to a FAT or ext system, but I want to be able to read and write from Windows; the last time I tried to read and write ext from Windows I had to install a plugin that didn't work out to well. BUT, I realize that macOS can only read NTFS ... maybe I should eventually change to a FAT-type format? <- I might do that eventually, but I already have lots of data on the drive, so I'm keeping it NTFS for now.

This command adds NTFS support for my Raspberry Pi:

```console
pi@raspberrypi:~ $ sudo apt-get install ntfs-3g
```

The drive is labeled as "TOSHIBA EXT."

```console
pi@raspberrypi:~ $ cd /
pi@raspberrypi:/ $ ls
bin   dev  home  lost+found  mnt  proc  run   srv  tmp  var
boot  etc  lib   media       opt  root  sbin  sys  usr
pi@raspberrypi:/ $ cd media
pi@raspberrypi:/media $ ls
pi
pi@raspberrypi:/media $ cd pi
pi@raspberrypi:/media/pi $ ls
SETTINGS  TOSHIBA EXT
```

I'll need to install the SAMBA implementation of the Server Message Block (SMB) protocol so that my Linux-based Raspberry Pi can share to Windows. Note that nothing gets installed below cause I already installed SAMBA for my print server project.

```console
pi@raspberrypi:/media/pi $ sudo apt-get install samba samba-common-bin
Reading package lists... Done
Building dependency tree       
Reading state information... Done
samba is already the newest version (2:4.5.16+dfsg-1).
samba-common-bin is already the newest version (2:4.5.16+dfsg-1).
samba-common-bin set to manually installed.
The following package was automatically installed and is no longer required:
  realpath
Use 'sudo apt autoremove' to remove it.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

I'm gonna make some changes to the samba configuration file, so I want to make a backup in case I break something.

```console
pi@raspberrypi:/media/pi $ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.old
pi@raspberrypi:/media/pi $ sudo nano /etc/samba/smb.conf
```

Here's what I added to the bottom of the configuration file.

```console
[1TB_Toshiba]
comment = Backup Folder
path = /media/USBHDD1/shares
valid users = @users
force group = users
create mask = 0660
directory mask = 0771
read only = no
```

The new configuration won't be up until I restart.

```console
pi@raspberrypi:/media/pi $ sudo /etc/init.d/samba restart
[ ok ] Restarting nmbd (via systemctl): nmbd.service.
[ ok ] Restarting smbd (via systemctl): smbd.service.
```

The drive is a network share now!

![drive-available-BUT-PASSWORD](/assets/2019-04-05-File_Server_Raspberry/drive-available-BUT-PASSWORD.PNG)

Here's how I added myself as a user with a password:

```console
pi@raspberrypi:/media/pi $ sudo useradd dave -m -G users
pi@raspberrypi:/media/pi $ sudo passwd dave
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

This adds me as a legit user in the samba system:

```console
pi@raspberrypi:/media/pi $ sudo smbpasswd -a dave
New SMB password:
Retype new SMB password:
Added user dave.
```

I restarted the system because it wouldn't let me logon after I ran the previous command.

```console
pi@raspberrypi:/media/pi $ sudo reboot
Connection to 192.168.1.3 closed by remote host.
Connection to 192.168.1.3 closed.
pi@raspberrypi:~ $ Connection to 192.168.1.3 closed by remote host.
Connection to 192.168.1.3 closed.
```

^The file still won't let me in ... weird. I'm gonna restart my computer. I have restarted my computer and rebooted my Raspberry Pi again and I still don't have access to my shared files ... why?

I blindly copied the following from the tutorial. See what I did wrong yet?

```
[Backup]
comment = Backup Folder
path = /media/USBHDD1/shares
valid users = @users
force group = users
create mask = 0660
directory mask = 0771
read only = no
```

^My path is wrong ... I just copied the path from the article.

Here's how I updated it to path = /media/1TB_Toshiba:

```console
pi@raspberrypi:~ $ sudo mkdir /media/1TB_Toshiba
pi@raspberrypi:~ $ sudo mount -t auto /dev/sda1 /media/1TB_Toshiba/
Mount is denied because the NTFS volume is already exclusively opened.
The volume may be already mounted, or another software may use it which
could be identified for example by the help of the 'fuser' command.
pi@raspberrypi:~ $ sudo fuser /dev/sda1
/dev/sda1:             871
pi@raspberrypi:~ $ sudo kill 871
pi@raspberrypi:~ $ sudo mount -t auto /dev/sda1 /media/1TB_Toshiba
pi@raspberrypi:~ $ sudo /etc/init.d/samba restart
[ ok ] Restarting nmbd (via systemctl): nmbd.service.
[ ok ] Restarting smbd (via systemctl): smbd.service.
pi@raspberrypi:~ $ sudo nano /etc/samba/smb.conf
pi@raspberrypi:~ $ sudo /etc/init.d/samba restart
[ ok ] Restarting nmbd (via systemctl): nmbd.service.
[ ok ] Restarting smbd (via systemctl): smbd.service.
```

... annnnddddd that fixed it! This is why it's important to read through everything ... don't commit to an action unless you know *exactly* what will happen! Here are my shared files!

![virtual-os-on-share](/assets/2019-04-05-File_Server_Raspberry/virtual-os-on-share.PNG)
