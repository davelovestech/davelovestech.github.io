---
layout: post
title:  "Creating a Perimeter Network"
date:   2024-02-14 16:38:00 -0800
categories: Firewall
---

The Sophos XG 135w Firewall that I'm using is vastly overpowered for home use. It's designed to handle small a small business or a branch office[^1]. So it has eight GbE copper ports! I'm currently only using two ports: LAN and WAN.  

![firewall-ports](/assets/perimeter-network/sophos-xg-135w.jpg)

So today I'll be setting up a new port for a screened subnet, a location I plan to host a web server from. I want my private network to be as secure as possible, so I don't want to allow any unrequested web traffic entering it. However, for a web server, I'll need to be accepting WAN traffic from site visitors. And I can allow outside web traffic in to the screened subnet and not be concerned about unwanted traffic on my private LAN because the screened subnet is on a completely different interface. Here's a good overview picture from some Okta documentation[^1]:

![dmz-overview](/assets/perimeter-network/dmz-overview.PNG)

I created a third firewall zone titled "DMZ" because that's the default, unchangeable, name for this port. A demilitarized zone (DMZ) was a term that was previously used for a perimter network. I have set the DMZ zone settings to allow for DNS, Ping, and Dynamic routing. It's a lot more limited than the access than my LAN has.

![dmz-zone-settings](/assets/perimeter-network/dmz-zone-settings.PNG)

And here's the single rule I created to allow devices on the DMZ to allow outgoing traffic to the WAN.

![dmz-to-wan-rule](/assets/perimeter-network/dmz-to-wan-rule.PNG)

I'll close this article out with the DHCP lease that my laptop was given after joinging that network.

![t520-dhcp-setting](/assets/perimeter-network/t520-dhcp-setting.PNG)


# References <a name="references"></a>
[^1]: [What Is a DMZ Network?][GV-link-name] 
[GV-link-name]: https://www.okta.com/identity-101/dmz/
