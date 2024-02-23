---
layout: post
title:  "Indentifying the Largest Directories on Linux"
date:   2018-02-19 22:42:40 -0700
categories: Linux
---

I am currently downloading several Operating System image files for Virtual Box. My Linux machine only has 16 GB of storage left and the downloads will take up more space than I have. A simple fix for this issue would be to identify the largest directories on my computer AND delete the directories that I do not need.

This terminal command will return the 10 largest directories on my system:

```console
[15:46] david@Ryzen ImageJ/ >  sudo du -a /var | sort -n -r | head -n 10
[sudo] password for david:
36912780	/var
36381420	/var/lib
31604008	/var/lib/mysql
31402572	/var/lib/mysql/23_Data
4274120	/var/lib/snapd
2993948	/var/lib/snapd/snaps
1277384	/var/lib/snapd/cache
445768	/var/cache
412384	/var/cache/apt
314416	/var/cache/apt/archives
```

I am no longer using the /var/lib/mysql directory, so I can delete it confidently. Here is how I deleted the directory:

```console
[15:47] david@Ryzen ~/ >  cd /var/lib
[15:47] david@Ryzen lib/ >  ls
AccountsService        fwupd            mysql          ubuntu-drivers-common
NetworkManager         gconf            mysql-files    ubuntu-release-upgrader
acpi-support           gems             mysql-keyring  ucf
alsa                   ghostscript      mysql-upgrade  udisks2
app-info               git              nssdb          update-manager
apparmor               hp               os-prober      update-notifier
apt                    initramfs-tools  pam            update-rc.d
aspell                 initscripts      plymouth       upower
avahi-autoipd          insserv          polkit-1       urandom
binfmts                ispell           python         ureadahead
bluetooth              libreoffice      resolvconf     usb_modeswitch
colord                 libxml-sax-perl  sgml-base      usbutils
dbus                   lightdm          snapd          vim
dhcp                   lightdm-data     snmp           whoopsie
dictionaries-common    locales          sudo           xfonts
doc-base               logrotate        systemd        xkb
dpkg                   man-db           tex-common     xml-core
emacsen-common         misc             texmf
flashplugin-installer  mlocate          ubiquity
[15:48] david@Ryzen lib/ >  sudo rm -rf mysql
```

That freed up a lot of space on my system! Now I am up to 43 GB of free space, so my downloads can continue without any worries.
