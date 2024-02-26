---
layout: post
title:  "Mounting a Linux Drive on Boot"
date:   2018-08-18 10:22:40 -0700
categories: Linux
---
I recently broke, and then fixed, my [Linux Swap File partition]({{ site.baseurl }}{% link _posts/2018-8-16-swap-not-available.markdown %}). It wasn't enough to create a new Swap File in Gparted. I also needed to edit the file located at /etc/fstab. 'fstab' isn't a directory; it's a filename that doesn't seem to have an extension. From what I understand, it automatically mounts all partitions within the file.

Fixing my Swap File reminded me of a problem that I've been having with another partition. I save all of my coding projects on another drive called 'Linux' and that has never automatically mounted. It's a minor annoyance to try and cd into a place that isn't there *every single time* that I boot up my computer. In the picture below, the top terminal window is me trying to cd into the drive that isn't mounted yet. The bottom window is successfully cd-ing into the partition *after* I opened the folder with my mouse.

![does not automount]({{"/assets/linux-automount-drive/cd_after_mount.jpg"}})

I don't like having to click the drive open with my mouse all the time, so I wondered if I could have it automount just like the Swap File. I found a StackExchange post that described [how to mount a drive on startup] and decided to give it a try. I needed to get the drive name with the 'fdisk' command and then change my '/etc/fstab' file to reflect that drive that I wanted to automount. As usual, my first attempt with a new command failed because I didn't use the right context:
```console
fdisk -l
```
```console
fdisk -l
fdisk: cannot open /dev/loop0: Permission denied
fdisk: cannot open /dev/loop1: Permission denied
fdisk: cannot open /dev/loop2: Permission denied
fdisk: cannot open /dev/loop3: Permission denied
fdisk: cannot open /dev/loop4: Permission denied
fdisk: cannot open /dev/loop5: Permission denied
fdisk: cannot open /dev/loop6: Permission denied
fdisk: cannot open /dev/loop7: Permission denied
fdisk: cannot open /dev/sda: Permission denied
fdisk: cannot open /dev/sdb: Permission denied
fdisk: cannot open /dev/sdc: Permission denied
fdisk: cannot open /dev/loop8: Permission denied
fdisk: cannot open /dev/loop9: Permission denied
fdisk: cannot open /dev/loop10: Permission denied
fdisk: cannot open /dev/loop11: Permission denied
fdisk: cannot open /dev/loop12: Permission denied
fdisk: cannot open /dev/loop13: Permission denied
```
I attempted to look at the drives with my basic permissions, so it blocked me. Using sudo is required to see this information:
```console
sudo fdisk -l
```
I've selected the important portion of the output:
```console
Disk /dev/sda: 111.8 GiB, 120034123776 bytes, 234441648 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x151779ce

Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1        2048 234438655 234436608 111.8G 83 Linux
```
One of the earlier commenters on the StackExchange article suggested to use the disk name, which would be '/dev/sda1' in my case. However, I remembered how a UUID was required for adding my Swap File, so I kept reading. Apparently you can specify the drive with the disk name or the UUID. I opted to use UUID, but I imagine I'd have the same results if I used disk name. The 'blkid' command is required to get UUID information.
```console
sudo blkid
```
Here's the relevant output:
```console
/dev/sda1: LABEL="Linux" UUID="363ce6d2-4ee3-4b3c-94cd-9d6c0cc0d816" TYPE="ext4" PARTUUID="151779ce-01"
```
I then opened the fstab file:
```console
cd /etc
sudo vi fstab
```
I've decided to copy over the file to have the text here. But, I couldn't figure out how to get copy and paste to work within the vi editor, so I decided to copy and paste the fstab file (it seems that I can't get access to copy the file ... I think playing with sudo might fix this, but I'm already wanting to move on). So, going off on a bit of a tangent, here's how I copied the fstab file:
```console
cd /etc
cp fstab /media/david/Linux/
```
Here's an image of the file (so I can get more practice writing in Markdown):
![fstab]({{"/assets/linux-automount-drive/fstab.jpg"}})
And here's the updated fstab file:
```console
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system>                        <mount point>          <type>  <options>         <dump>  <pass>
# / was on /dev/sda3 during installation
UUID=94e06f32-ec74-4c76-928e-a79d539f9214    /                 ext4    errors=remount-ro  0       1
# swap was on /dev/sda5 during installation
# this was old swap location ... I think, lol
# UUID=e58e8cbb-b343-4c97-8dde-053f8ad3a631 none               swap    sw                 0       0
# this is new swap location according to 'sudo blkid'
# ... I hope this works, haha
UUID=e25632c6-59ad-431c-bb9a-0f14a6bc4cfd none                 swap    sw                 0       0
UUID=363ce6d2-4ee3-4b3c-94cd-9d6c0cc0d816 /media/david/Linux   ext4   defaults            0       0
```
Look at that last line (the one with a mount point of /media/david/Linux). That's the new line that I changed in an attempt to get automount to work. I've read that you can do serious damage to your system by changing fstab, so I carefully read over my chosen syntax a few times before saving ... hahaha, I don't wanna get lost in another new cycle of spending hours to fix Linux again!

![automount works]({{"/assets/linux-automount-drive/drive_now_automounts.jpg"}})

It worked! After saving fstab and restarting, I attempted to cd into the partition and I had no problems! Hooray! I'm learning more about Linux *and* I will now be saving a few extra seconds on each bootup because I don't need to use my mouse to click open the drive!

[how to mount a drive on startup]:https://askubuntu.com/questions/154180/how-to-mount-a-new-drive-on-startup
