---
layout: post
title:  "Running Windows 10 on VirtualBox"
date:   2019-1-22 22:42:40 -0700
categories: Windows Software
---

I intend to gain experience across Windows, Linux and Mac operating sytems. I'm told that Linux is the main operating system for computational work, so I'll be mainly using Linux. I will be installing different operating systems on my Linux machine witht he help of VirtualBox. This post is about how I got Windows 10 to run on my Linux operating system.

A free developer version of Windows 10 [is available here.](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) Here is how I unzipped the Windows 10.zip file:

```console
[17:27] david@Ryzen VirtualBox VMs/ >  ls          
IE11.Win7.VirtualBox.zip                macOS Mojave HFS by Techsviewer.rar
IE11.Win81.VirtualBox.zip               macOS Mojave HFS by Techsviewer.vmdk
MSEdge.Win10.VirtualBox.zip             multitest_test1_1532241391169_15593
Mojave                                  multitest_test2_1532241435435_54465
localusers_default_1532706434247_84824  testbox01_default_1532223940091_73006
[17:33] david@Ryzen VirtualBox VMs/ >  unzip MSEdge.Win10.VirtualBox.zip
Archive:  MSEdge.Win10.VirtualBox.zip
  inflating: MSEdge - Win10.ova      
```

Apparently you can't create a new virtual machine with the free Windows 10.ova file. This is the error message I got:

```console
Failed to open the disk image file /media/david/Terabyte/VirtualBox VMs/MSEdge.Win10.VirtualBox.zip.
```

I didn't realize that I needed to import the virtual appliance. Luckily VirtualBox took care of that once I double clicked the Win10.ova file. It installed with ease once I let VirtualBox take over. The first machine start yielded a black screen, which was concerning ... BUT, I restarted it and then it worked fine! Here's Windows 10 inside Linux:

![Win10_VirtualBox](/assets/VirtualBox_Pics/Win10_VirtualBox.png)
