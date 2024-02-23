---
layout: post
title:  "Creating an IoT Devices Wi-Fi Network"
date:   2024-02-18 12:03:00 -0800
categories: Networking
---
I have a variety of Internet of Things (IoT) devices, such as smart lights, Amazon Alexa, and Google Voice. I love the added functionality that these devices bring. But I don't want them to have access to my LAN. The main reason for this is that I don't know the frequency of security patches that these devices get. And a compromised IoT device could be the gateway to my home network. I also have concerns about what kind of data these devices may be collecting from my network.

I am not alone in this concern. Having a separate network for IoT devices is a standard cybersecurity practice. Here's a good introduction to this issue: [Internet of Things (IoT)](https://www.fbi.gov/contact-us/field-offices/portland/news/press-releases/tech-tuesday-internet-of-things-iot). So in this article, I'll be creating a separate IoT network that only has outgoing permission to the WAN. 

I've talked about limiting Zone Network Services and shown screenshots of the Sophos network interface creation on previous posts, so let's jump straight to DHCP settings. You'll find a screenshot of my DHCP settings below. I've redacted some IP settings for security reasons. I have created room for up to 24 devices to get assigned an IP address by DHCP.

![DHCP-settings](/assets/iot-devices-network/DHCP-settings.PNG)

I've skipped over the boring part of adding all of my devices to the Wi-Fi. You'll find the full list of DHCP leases for my IoT devices network below. I have redacted the IP addresses and the last six digits of the device MAC addresses for security reasons.

![DHCP-lease-info](/assets/iot-devices-network/DHCP-lease-info.PNG)

Why did I only redact the last six digits of the device MAC addresses? Well, that's because the first 6 digits are the somewhat common Organizationally Unique Identifier (OUI). And the last six digits are unique to the specific IoT devices. Here's a good picture from NetWork Encyclopedia[^1] that shows this:

![mac-address-format](/assets/iot-devices-network/mac-address-format.PNG)

I created one rule for this network to allow any outgoing WAN traffic. So my IoT devices can reach out to the internet, but they are not able to connect to my LAN. I feel safer already!

![iot-to-wan-rule.PNG](/assets/iot-devices-network/iot-to-wan-rule.PNG)


# References <a name="references"></a>
[^1]: [Network Encyclopedia MAC Address][GV-link-name] 
[GV-link-name]: https://networkencyclopedia.com/mac-address/





