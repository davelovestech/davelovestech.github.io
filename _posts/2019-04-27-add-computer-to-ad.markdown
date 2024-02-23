---
layout: post
title:  "Adding a Computer to a Windows AD Domain"
date:   2019-04-27 4:00:40 -0700
categories: Active_Directory
---

I added a user to the Active Directory in the previous article, so now it's time to add a computer. This'll require another operating system! I downloaded an [Evaluation of Windows 10 Enterprise here.](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise) Here's one of the earlier images from getting the VirtualBox instance running:


![win-10-enterprise-system-install](/assets/2019-04-06-add-group-policy/win-10-enterprise-system-install.PNG)

Once I got the new Windows 10 Enterprise instance up and running I added it to the example.com domain that the Windows Server 2012 AD instance is running:

![member-of-exampleDOTcom-domain](/assets/2019-04-06-add-group-policy/member-of-exampleDOTcom-domain.PNG)

I used the jared administrator instance as the credentials for joining the example.com domain.

![login-computer-name-jared](/assets/2019-04-06-add-group-policy/login-computer-name-jared.PNG)

Changing a domain requires a restart, so I rebooted the system before making any other changes. Remember how this was in the Windows 10 Enterprise system? Well, now it's time to switch over into the Windows Server 2012.

I went into the Active Directory Administrative Center > Lab > Computers and found my Windows 10 Enterprise OS as a new computer. You can tell it's the same OS cause it has the matching name of "lab-win-10-01" from above:

![computer-nowIN-computers](/assets/2019-04-06-add-group-policy/computer-nowIN-computers.PNG)
