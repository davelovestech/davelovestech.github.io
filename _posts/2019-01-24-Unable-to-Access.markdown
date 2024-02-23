---
layout: post
title:  "Unable to Access Shared Drive"
date:   2019-1-24 13:42:40 -0700
categories: Troubleshooting
---

Clicking the Terabyte HDD icon on the launcher makes the icon blink, like it was going to open, but then it doesn't open. Attempting to open the drive from a file explorer yields this error message:

```console
Unable to access “Terabyte”
Error mounting /dev/sdc1 at /media/david/Terabyte1: Command-line `mount -t "ntfs" -o
"uhelper=udisks2,nodev,nosuid,uid=1000,gid=1000" "/dev/sdc1" "/media/david/Terabyte1"' exited
with non-zero exit status 14: The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Failed to mount '/dev/sdc1': Operation not permitted
The NTFS partition is in an unsafe state. Please resume and shutdown
Windows fully (no hibernation or fast restarting), or mount the volume
read-only with the 'ro' mount option.
```
[this Stack Exchange post](https://askubuntu.com/questions/145902/unable-to-mount-windows-ntfs-filesystem-due-to-hibernation) says that this error can happen if a Windows partition was hiberated before Linux tried to open it. I don't think my Windows parition was hibernated the last time that it was used ... I'm pretty sure I turned it off normally. **However**, I just recovered from a Linux Emergency Mode situation, so perhaps the issues are related?

I see that it's suggested for me to boot up the Windows partition and then turn it off normally before going back into Linux. But, I am wondering if it's possible to fix this situation while logged in to Linux.

coyping this "mount -t ntfs-3g -o ro /dev/sda3 /media/windows" for my needs
```console
[16:20] david@Ryzen david/ >  sudo mount -t ntfs-3g -o ro /dev/sdc1 /media/david/Terabyte
```

That worked to get my filesystem back up in read only mode, but I want to be able to write too! I'm trying to get macOS Mojave to work in VirtualBox and the virtual hard drive is stored on the Terabyte drive that is read only. I can't boot the Mojave in VirtualBox until I get writes allowable on the hard drive.

![VirtualBox_Failed](/assets/Shared_Drive/virtualbox_failed.png)

I could do a bit more reading on the "ntfs-3g" command with its manual:

```console
man ntfs-3g
```

It looks like I could use remove-hiberfile to delete the windows hibernation file without turning off my Ubuntu system. I don't have any important information on the Windows partition, so I'm fine with this.

```console
remove_hiberfile
       Unlike in case of read-only mount, the read-write mount is denied if the NTFS volume is
       hibernated. One needs either to resume Windows and  shutdown  it  properly,
       or use this option which will remove the Windows hibernation file. Please note, this
       means that the saved Windows session will be completely lost. Use
       this option under your own responsibility.
```

I attempted to do taht with these terminal entries, but I couldn't get it to work. I believe that my command entry is correct because it matches the syntax of what I've read about online.

```console
[16:35] david@Ryzen david/ >  sudo umount /dev/sdc1
[16:35] david@Ryzen david/ >  sudo mount -t ntfs-3g -o remove_hiberfile /dev/sdc1 /media/david/
Terabyte
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Failed to mount '/dev/sdc1': Operation not permitted
The NTFS partition is in an unsafe state. Please resume and shutdown
Windows fully (no hibernation or fast restarting), or mount the volume
read-only with the 'ro' mount option.
```

I guess I will try to turn off the computer, boot into Windows, shut that down and then boot back into Linux. I tried that and it didn't work ... this is weird. [Here's a StackExchange post](https://askubuntu.com/questions/462381/cant-mount-ntfs-drive-the-disk-contains-an-unclean-file-system) about how to use "ntfsfix" to address this kind of situation. I adjusted the command for my situation and it worked!

```console
[19:00] david@Ryzen ~/ >  sudo ntfsfix /dev/sdc1
Mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors...
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/sdc1 was processed successfully.
```

It was mounting to "/media/david/Terabyte1" instead of "/media/david/Terabyte". I unmounted and then remounted with my desired location like so:

```console
[19:05] david@Ryzen ~/ >  sudo umount /dev/sdc1
[19:05] david@Ryzen ~/ >  sudo mount -t ntfs-3g -o rw /dev/sdc1 /media/david/Terabyte
```

This got it working and at the right location!
