---
layout: post
title:  "Running Windows 7 on VirtualBox"
date:   2019-01-25 17:20:40 -0700
categories: Windows
---

A free developer version of Windows 7 [is available here.](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) Here is how I unzipped the Windows 10.zip file:

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

I'll be running the virtual machine in Oracle VM VirtualBox Manager. I won't go into installing this program, but I will note that you'll need to install the VirtualBox Extension Pack to get access to virtual USB support.

Getting Windows 7, 8, and 10 to work is pretty easy. All you need to do is import the "appliance" from the VirtualBox main window. Here are the provided settings for the machine:

![Windows7_Appliance](/assets/Virtual_Windows/Windows7_Appliance.png)

It worked on the first boot! Here's my Ubuntu 16.04 system with a virtual Windows 7 alongside.

![Windows_7](/assets/Virtual_Windows/Windows_7.png)
