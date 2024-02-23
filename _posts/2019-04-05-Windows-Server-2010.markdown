---
layout: post
title:  "Enabling Active Directory on Windows Server 2012"
date:   2019-04-05 1:00:40 -0700
categories: Active_Directory
---

This is the first in a series of posts about Active Directory (AD). Active Directory is a collection of tools for managing computer systems and users within a domain. I'm gonna be creating an AD with these guest operating systems:

* Windows Server 2012
* pfSense (a firewall)
* Windows 10 Enterprise

^ I'll be running all of this out of VirtualBox on my Windows 10 Virtualization PC. **Before I continue, I want to remind you of the importance of using software legally! I am using free evaluation copies of Windows to do this. Don't break the law.** I won't be documenting every individual step with great detail because there's already a [great tutorial for Windows 2012 AD here.](https://www.psattack.com/articles/20160718/setting-up-an-active-directory-lab-part-1/) It's the tutorial that I followed for making my Active Directory.

I grabbed my legal evaluation copy of [Windows Server 2012 here.](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2) I used VirtualBox to create a Guest OS with 1 CPU, 1024 MB of RAM and 30 GB of hard drive space. Here's the name and location info for my brand new guest OS:

![new-os](/assets/2019-04-06windows-server-2012/new-os.PNG)

Here's how I added the ISO for the install disk:

![iso-start-up-file](/assets/2019-04-06windows-server-2012/iso-start-up-file.PNG)

You could use the default Network Address Translation (NAT) setting; that'd create a virtual network between the real network and the virtual machines. I have chosen Host-Only to stop VirtualBox from acting as a router. This keeps the virtual machines from this experiment isolated to their own network. Here are those settings:

![host-network-manager](/assets/2019-04-06windows-server-2012/host-network-manager.PNG)

^Note that 10.10.10.1 is the IP address of the Windows Server that I'll be using throughout all of the AD labs. Now that the network is set up I can install Windows Server 2012. It's alive!

![windows-2012-startup](/assets/2019-04-06windows-server-2012/windows-2012-startup.PNG)

There were several potential flavors of Windows Server 2012 that I could have chosen. I went with Windows Server 2012 R2 Standard Edition (Server with a GUI). Sadly I'm not yet talented enough to do things entirely on the command line.

I installed Virtualization Guest Extensions before moving on.

![install-virtual-box-guest-additions](/assets/2019-04-06windows-server-2012/install-virtual-box-guest-additions.PNG)

Remember that 10.10.10.1 IP address that I picked in the VirtualBox Host Manager? Here I'm setting Windows Server 2012 to have that same IP address.

![giving-win2012-ipaddress](/assets/2019-04-06windows-server-2012/giving-win2012-ipaddress.PNG)

I've named this computer "lab-dc01". I'll be restarting before moving in with the Active Directory setup.

![computer-named-lab-dc01](/assets/2019-04-06windows-server-2012/computer-named-lab-dc01.PNG)

Now it's time to start working on Active Directory! See how "File and Storage Services" is the only thing under "Roles and Server Groups"? It's time to add some things! :)

![server-manager-dashboard](/assets/2019-04-06windows-server-2012/server-manager-dashboard.PNG)


I clicked "Add Roles and Features" and selected "Active Directory Domain Services." This is what'll be built upon to gain Active Directory features!

![add-active-directory-domain-services](/assets/2019-04-06windows-server-2012/add-active-directory-domain-services.PNG)

I didn't get a picture of this ... but there'll be a yellow flag in the upper right. That's how to promote the server to a domain controller. The server had the power to do AD, but this promotion is what enables it to do so. Don't be confused by the term "forest" ... it's just the fancy AD word for the collection of computers. I went with "example.com"

![add-new-forest-exampleDOTcom](/assets/2019-04-06windows-server-2012/add-new-forest-exampleDOTcom.PNG)

Oh no! It's a confusing error!

![dns-delegation-no-parent](/assets/2019-04-06windows-server-2012/dns-delegation-no-parent.PNG)

^ Don't worry! It can't find the DNS yet ... because the DNS hasn't been made yet! I just clicked on through :)

I rebooted the system and ... LOOK! Active Directory and DNS are now enabled roles!

![ad-ds-enabled](/assets/2019-04-06windows-server-2012/ad-ds-enabled.PNG)
