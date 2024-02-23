---
layout: post
title:  "Opening Firewall Ports for Google Voice"
date:   2024-02-12 17:22:00 -0800
categories: Firewall
---
I set up a new firewall on the edge of my network on previous posts. So far I have only added rules to allow outgoing Ping, HTTP and HTTPS traffic. I just tried using Google Voice and it didn't work. So now is the time to create a new firewall rule! 

The documentation for Google Voice[^1] indicates that I need to open the following ports: 

* Outbound UDP ports range: 19302–19309
* Outbound UDP ports range: 26500–26501
* Outbound TCP port 443

I have already opened up TCP port 443 for outgoing HTTPS traffic on a previous post, so I only need to focus on the two outgoing UDP rules. I'm going to stick with the Sophos default of ports 1 - 65,535 being potential options for the source port. There are two separate port ranges that are required, so I'll need to create two different services entries. Here's the entry for the first range:

![Cloudflare-Proxy](/assets/google-voice-ports/google-voice-udp-ports.PNG)

I added both of those ranges to a single rule for Google Voice. 

![Cloudflare-Proxy](/assets/google-voice-ports/google-voice-both-udp-rules.PNG)

Once I enabled that rule, I was able to start using Google Voice for calls again! So, to recap, I am now up to having four total firewall rules. Note: the rule IDs are out of order because I recreated the initial firewall rules for the screenshot.

![Cloudflare-Proxy](/assets/google-voice-ports/firewall-rule-google-voice.PNG)

# References <a name="references"></a>
[^1]: [Voice connectivity requirements][GV-link-name] 
[GV-link-name]: https://support.google.com/a/answer/9206518?hl=en#zippy=%2Cvoice-architecture%2Coutbound-ports-need-to-allow-voice-traffic%2Cbandwidth-requirements-per-site%2Callow-list-uris
