---
layout: post
title:  "test post"
date:   2024-02-25 12:38:00 -0800
categories: macOS
---
# Mac General Information
There is a lot of information to be found within the the About section. Here is how to do that: Launchpad > System Settings > General > About. It contains the following information:
 
* Name
* Chip
* Memory
* Serial Number
* macOS Version
* Storage

As you can see from the screenshot below, I am on a Mac mini named LA751 running macOS Ventura. And that it has an Apple M1 chip, 8 GB of RAM, and a 4 TB hard drive. I have redacted the serial number for security reasons. 

![macOS_General_Info](/assets/mac-system-info/macOS_General_Info.PNG)

# Ethernet Connection Settings
Internet settings can be found here: Launchpad > System Settings > Network > Ethernet. The information that you'll find here is: 

* Computer IP address
* Subnet Mask
* Router IP Address
* DNS Servers

This is the first place I'd check if the computer was having internet connectivity issues; this or the wi-fi settings. I have redacted the first two octets of the IP address and the router for security reasons. 

![mac-ethernet-details](/assets/mac-system-info/mac-ethernet-details.PNG)

Before going on to the next ethernet detail, I'd like to talk a bit about the network interface controller (NIC). It's the physical hardware that's responsible for handling the wired ethernet connection. And it has a 12 digit hexadecimal number called a MAC, or medium access control address. The MAC address is not at all related to the "mac" part of "macOS"; it is a unique address that is only found on this particular piece of hardware. The MAC address for this NIC starts with 14:98:77. The MAC address information can be found here: Launchpad > System Settings > Network > Ethernet > Details > Hardware.

![ethernet-mac-address](/assets/mac-system-info/ethernet-mac-address.PNG)

I have redacted the last six digits of the MAC address because that would identify the NIC of this particular computer. The first six digits are referred to as the organizationally unique identifier; they identify the manufacturer. And the last six digits are specific to the particular network interface controller. 

One way that a MAC address is useful is with identifying unknown network devices. Say you've got a wi-fi device on your network that doesn't respond to pings and you don't know where it is. You could do a MAC address lookup to determine the manufacturer. The MAC address lookup for this device can be found below. This has been useful to me lately because I wanted to identify all of the devices on my wi-fi network. I went device by device to identify the MAC addresses for my smart lights, my smart plugs, and my Echo dot. And that helped me identify which devices I should be turning on/off to determine device-specific MAC addresses.

![mac-address-lookup](/assets/mac-system-info/mac-address-lookup.PNG)

The main way that MAC addresses have been important to me is with network access control. One of my tasks at work has been to add MAC addresses to the allow list for wi-fi. For me personally, I used to have a MAC allow list of my wi-fi devices on my router. I recently transitioned to using a Sophos XG 135w firewall. It has wireless functionality! So now I'm using a MAC address allow list on my firewall.