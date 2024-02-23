---
layout: post
title:  "Setting Up my Firewall"
date:   2024-02-10 21:54:00 -0800
categories: Firewall
---
 
 The Sophos XG 135w Firewall free Base License includes Stateful Firewall, VPN, Wireless[^1]. And one that was going to be decommissioned was given to me as a gift from a previous job! It was of course wiped of all data and I wiped it again before using it just to be on the safe side. And I've decided to jump right it and set it up as the firewall for my home network! It has eight GbE copper ports and today I'll be setting up two of those ports.

![firewall-ports](/assets/screened-subnet/sophos-xg-135w.jpg)

I've skipped over the command line wiping step and the initial web-based device configuration in order to focus on more interesting things. Before setting up my firewall interfaces, I think it's important to mention that the only firewall rule I currently have is the default to drop everything. I plan to keep my firewall as secure as I can, so I'll only be opening up ports to get services to work.

![drop-all-firewall-rule](/assets/screened-subnet/drop-all-firewall-rule.PNG)

Next was setting up the wide area network (WAN) interface. This will be my connection to the internet. I selected the proper IP settings to connect to my ISP. Note: I'll be redacting a lot of the settings out of a concern for security.

![port-2-wan.PNG](/assets/screened-subnet/port-2-wan.PNG)

And I also set up another port for my local area network (LAN), which is my private home network. 

![lan-and-wan-ports](/assets/screened-subnet/lan-and-wan-ports.PNG)

I think it's important to point out that I have disabled all of the device access options from the WAN. This is to make sure that the only place that this firewall can be administered from is my LAN. 

![disabled-all-services-from-wan](/assets/screened-subnet/disabled-all-services-from-wan.PNG)

The Drop All rule is still in place, so nothing should be able to get in or to leave my network. My first rule was to allow outgoing Ping (ICMP) packets. Here's a screenshot of me ping-ing the Google DNS before and after I allowed Ping traffic. You'll see that the Pings start working after the sixth attempt. 

![ping-once-allowed](/assets/screened-subnet/ping-once-allowed.PNG)

So, there you have it! I now have have a LAN port and a WAN port and outgoing Ping traffic is the only thing that is currently allowed.

# References <a name="references"></a>
[^1]: [Sophos Firewall Licensing][GV-link-name] 
[GV-link-name]: https://docs.sophos.com/nsg/sophos-firewall/19.5/Help/en-us/webhelp/onlinehelp/AdministratorHelp/Administration/Licensing/index.html#module-subscription-details
