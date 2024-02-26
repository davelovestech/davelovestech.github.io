---
layout: post
title:  "Fixing a Linux Master Boot Record"
date:   2018-08-16 11:57:40 -0700
categories: Troubleshooting
---
It was bound to happen eventually ... I finally broke my Master Boot Record! For those that haven't been following my other posts, I've been running a dual-boot system of Ubuntu 16.04 and Windows 10 for a few months now. I had been a Windows user since 95, but I'm learning Linux because it's one of the more popular OS environment for software development. I initially gave my Ubuntu '/' directory 50 GB of space, but I've since used all but 1 GB of it!

I decided to just add more space, so I booted from my Ubuntu live USB stick and ran the Gparted utility. It wouldn't let me add more space to my '/' directory because Windows was immediately to the left and my Ubuntu Swap File was immediately to the right. I first made a copy of my '/' to recover from if I failed and then I deleted my Swap File (to make room for resizing Ubuntu). I *could* have just resized my Ubuntu '/' by adding size to the right, but I didn't want to deal with a resizing issue if I wanted to make my Windows partition bigger in the future.

I first decided to create a 20 GB Swap File on the right-most section of my hard drive. Then I copied my 50 GB copy of my Ubuntu '/' directory directly to the left of the Swap File *AND* I resized the 50 GB partition to 100 GB. I saved changes and ran them and thought I'd be done with it ... I was *INCREDIBLY WRONG*, hahaha. I turned off my computer, removed the USB of Ubuntu live and tried a clean boot and was greeted with an error message of "error: no such partition". Sure, there was a terminal available with a starting phrase of "grub rescue>", *BUT* that terminal wouldn't recognize any commands!
![no such partition]({{"/assets/no-such_partition/no_such_partition.jpg"}})

Some quick Googling pointed my in the direction of this [Ubuntu Forums 'lilo' Recommendation]. It was recommended that I install lilo and use it to fix my Master Boot Record, so I followed the instructions and was ready to be done! The terminal commands (from below) were suggested:

```console
sudo apt-get install lilo

sudo lilo -M /dev/sda mbr
```

*BUT*, there was also a cautionary suggestion to check that your Master Boot Record was on the sda drive ... so I did that:
![lilo correct drive]({{"/assets/no-such_partition/lilo_correct_drive.jpg"}})

Checking on the drive was a good call because my Master Boot Record (MBR) was actually on sdc! I think this is because, when I built my computer, I randomly connected the SATA cables. I could've been careful to have the MBR drive on the SATA 0 port, but I had already installed my video cord ... out of laziness I just plugged in wires without looking at what I was doing! Hahaha, I thought it wouldn't be a problem because I could just change the recommended terminal commands to use sdc, like this:

```console
sudo apt-get install lilo

sudo lilo -M /dev/sdc mbr
```

![install lilo]({{"/assets/no-such_partition/install_lilo.jpg"}})

Again, I thought I had fixed the problem! I was ready to start coding again, but I was greeted with the same "error: no such partition" screen! I did some more Googling and found a new Stack Exchange post that suggested using [EasyBCD from within Windows], so I tried doing that. On my first try I added Linux to the Boot Menu with a Boot Type of 'GRUB (Legacy)'; I didn't intentionally do that. That was just the pre-selected option.

![BC2 Windows Fix Attempt]({{"/assets/no-such_partition/BC2_bootloader_adding_linux.jpg"}})

I restarted and was excited when I was greeted with the new BC2 bootloader screen!
![BC2 Bootload Image]({{"/assets/no-such_partition/BC2_bootloader_fail.jpg"}})

Again, I thought I'd fixed the problem. **HOWEVER**, selecting 'Ubuntu' just lead to my computer restarting! Windows worked fine, but I don't really care about Windows ... these days all I use Windows for is Microsoft PowerPoint ... I need to learn how to use Windows within a Linux Virtual Box! Anyways, enough of this tangent!

I tried adding my Ubuntu Live USB stick and restarting into Linux Live, but that wouldn't work either! My computer was not being responsive to F2, F11 or Del ... it wouldn't even respond to combinations of those keys! WHAT?!? I have never been unable to change my default boot order before ... I had no idea what was going on ... I figured that the Windows BC2 Bootloader program may have disabled those keys, so I tried a variety of BC2 options:
* unchecked 'Use BC2 Bootloader'
* tried having Ubuntu as the default
* tried having Windows as the default
* tried having no default
* tried deleting the Linux MBR record and re-loading it

None of those options worked! And then I tried to do something silly: **I swapped my keyboard USB connection with my Ubuntu Live USB connection** and that fixed my F2/F11/Del issue! I think it may have been because my keyboard was on a USB 2.0 connection and my USB stick was on a USB 3.0 connection, but I never bothered figuring it out. The important thing was that I could now use F11 to boot from my Live Linux USB stick!

Once I got Ubuntu Live to boot, I tried this How-To-Geek article about [restoring the GRUB2 loader]. Here are the terminal commands that I ran:

```console
sudo apt-add-repository ppa:yannubuntu/boot-repair

sudo apt-get update

sudo apt-get install -y boot-repair

boot-repair
```
And that did it! I restarted my computer and was greeted by this beautiful Ubuntu Grub screen!
![Ubuntu Grub Works]({{"/assets/no-such_partition/ubuntu_bootloader_works.jpg"}})

When this all started, I thought all I'd need to was use Gparted from a live Ubuntu USB session to make my '/' directory bigger. *However*, I ended up spending the better part of 2 hours troubleshooting a series of weird errors. I figured out how to get things working through experimentation. That's always been the best way of solving problems for me.
* Google the error
* attempt the first suggestion that makes sense
* if that doesn't work, try the next suggestion
* if that doesn't work, start playing:
  * what does this check box do?
  * would a different key combination do something?
  * use the Windows utility
  * try switching cables

^ I never thought switching cables would work ... it was just this random idea that I had and it worked. I like to think of this method of troubleshooting as iterative, creative experimentation. Sure, I has a lot of other things planned today. And, I definately found myself getting angry at times, but the important thing is that I learned a lot today!

[Ubuntu Forums 'lilo' Recommendation]:https://ubuntuforums.org/archive/index.php/t-1359802.html
[EasyBCD from within Windows]: https://askubuntu.com/questions/747572/dual-boot-only-boots-windows
[restoring the GRUB2 loader]:https://www.howtogeek.com/114884/how-to-repair-grub2-when-ubuntu-wont-boot/
