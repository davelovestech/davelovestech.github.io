---
layout: post
title:  "Setting up pfSense and DHCP for a Windows Server"
date:   2019-04-05 2:00:40 -0700
categories: Firewall
---

# Setting up a pfSense Firewall

This post is a continuation of my demo of a Windows 10 Enterprise client computer running on a Windows 2012 Server Active Directory setup. I'll be setting up a pfSense firewall and a Dynamic Host Configuration Protocol (DHCP) in this post.

This Active Directory setup is running on a Host-Only Adapter, so it's important to ensure all guest operating systems are playing on the same "VirtualBox Host-Only Ethernet Adapter #2" that the Windows Server is using. Here's the setting that I used before starting the guest OS install of pfSense.

![second-network-adapter-for-freebsd](/assets/2019-04-06-pfsense-and-dhcp/second-network-adapter-for-freebsd.PNG)

None of the systems I'll add to the Windows Server 2012 Active Directory setup will have internet access because of the Host-Only Adapter setting. The pfSense Firewall system, that I am about to setup, will properly route between the Host-Only Network and the Host Computer's Network.

I used 1 CPU, 256 MB of RAM, and 5 GB of HDD for pfSense. Here's that early install page:


![pfsense-bsd-64-bit-creation](/assets/2019-04-06-pfsense-and-dhcp/pfsense-bsd-64-bit-creation.PNG)

I used [pfSense ISO from their website](https://www.pfsense.org/download/) to initiate this virtual machine. Here's an early image of the install process:


![pfSense-installer](/assets/2019-04-06-pfsense-and-dhcp/pfSense-installer.PNG)

Windows doesn't mind if the original ISO is still present on first boot, but it'll confuse pfSense. Make sure that you remove the ISO after install.

Here's what pfSense looks like after it finishes booting. It needs to learn the interfaces it'll use for being a firewall. I selected 2 for "Set interface(s) IP address" and then 2 again for "LAN Interface."

![change-ip-addressing-landing-page](/assets/2019-04-06-pfsense-and-dhcp/change-ip-addressing-landing-page.PNG)

Here are the settings that I selected:
* New LAN IPv4 Address of 10.10.10.2
* New LAN Subnet Bit Count of 24

I didn't bother with adding an upstream IP for a LAN interface or an IPv6, or a local DHCP server (the Windows Server will be doing that).

# Installing DHCP on Windows Server 2012
It's time to head back into the Windows Server 2012 system. I'll be setting up a DHCP system for the server. Domain Controllers are usually just running the domain, but small networks (like this one) run DHCP.

I went into "Add Roles and Features" and installed the DHCP Server Role. There was a lot of clicking next (just like when I installed Active Directory).

![dhcp-server-role](/assets/2019-04-06-pfsense-and-dhcp/dhcp-server-role.PNG)

There was a yellow flag in the upper right (just like with the Active Directory install), so I went over there and selected "Complete DHCP configuration."

I selected Tools > DHCP > "lab-dc0.example.com" > IPv4 > Add New Scope. I went with "Lab" as the name of the DHCP scope and a range of IP addresses from 10.10.10.100 to 10.10.10.199. I used 10.10.10.2 as the IP address for the domain controller (the IP address of pfSense).

![adding-ip-address-for-gateway](/assets/2019-04-06-pfsense-and-dhcp/adding-ip-address-for-gateway.PNG)

My Windows 2012 Server now has a new role of DHCP. Cool!

![dhcp-server-added](/assets/2019-04-06-pfsense-and-dhcp/dhcp-server-added.PNG)
