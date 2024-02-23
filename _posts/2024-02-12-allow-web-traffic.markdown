---
layout: post
title:  "Allowing Outgoing Web Traffic"
date:   2024-02-12 16:38:00 -0800
categories: Firewall
---
I set up a Sophox XG 135w Firewall on the edge of my home network on my previous network. And outgoing Ping traffic is the only thing that is currently allowed. Obviously, I wasn't able to access any websites so I went about addressing that first.  

I started by defining services for Hypertext Transfer Protocol (HTTPS). A screenshot of the service for HTTPS can be found below. I have left the Sophos source port default of 1-65,535 as-is. The other settings were chosen because HTTPS is a TCP protocol that uses port 443. 

![https-service](/assets/http-and-https-rules/HTTPS-port.PNG)

The Service is part of the settings for the broader rule. The rule I created, that you will see below, was for traffic from my Local Area Network (LAN) to the Wide Area Network (WAN). This will allow outgoing HTTPS traffic. I do not need to create a rule to allow WAN to LAN HTTPS traffic because the Sophos XG 135w firewall is a Stateful Firewall[^1]; it keeps track of connections and will allow incoming traffic from a connection that is already established.

![https-rule](/assets/http-and-https-rules/https-source-and-destination-zones.PNG)

I created rules to allow both HTTPS (443) and HTTP (80) traffic. Note: please ignore the rule ID number! I have been playing with a lot of test rules and those are skewing the ID number.

![firewall-rule](/assets/http-and-https-rules/current-firewall-rules.PNG)

As you can see by the firewall log traffic below, I am now able to access websites using both the HTTP and HTTPS protocols. I have redacted the source and destination IP addresses for security reasons.

![firewall-logs](/assets/http-and-https-rules/sophos-firewall-web-traffic-log.PNG)

# References <a name="references"></a>
[^1]: [Sophos Firewall Licensing][GV-link-name] 
[GV-link-name]: https://docs.sophos.com/nsg/sophos-firewall/19.5/Help/en-us/webhelp/onlinehelp/AdministratorHelp/Administration/Licensing/index.html#module-subscription-details