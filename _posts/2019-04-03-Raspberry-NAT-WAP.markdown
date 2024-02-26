---
layout: post
title:  "Creating a WAP with NAT on Linux."
date:   2019-04-04 12:00:40 -0700
categories: Networking Linux
---

I have been using a Netgear N600 router with Dual Band Wi-Fi (2.4 & 5 GHz) for a little over a year now and I don't have any problems with it :) However, I've been reading about Wi-Fi network interference lately ... this has inspired me to start playing with Wi-Fi interference with my home lab. I recently purchased a Raspberry Pi 3B+ that has Dual Band Wi-Fi, so I now have the perfect opportunity to start playing :)

But, before I can start testing out Wi-Fi interference, I'll need to set up a Wireless Access Point for my Pi. This is actually a pretty involved process, so I'll be working from [this tutorial.](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md) Before I continue, I'd like to explain some concepts that will be used throughout this project.

Iptables are a set of rules used by a Linux firewall to determine how packets (data) get treated. Network Address Translation (NAT) allows a group of private IP addresses to be treated as a single IP address when communicating with the internet; I'll eventually talk about the limited IP addresses for IPv4 and why it's important ... you could Google that if you don't already know about it. Finally, in the context of NAT, masquerade is an algorithm used to convert between the private IP address space and the publicly assigned IP address.

Coming back to the tutorial, the first thing I'll be doing is making sure my Raspbian OS is up-to-date.

```console
sudo apt-get update
sudo apt-get upgrade
```

Next up, I'll be grabbing the software packages that are required for creating a WAP with a Raspberry Pi: dnsmasq & hostapd. Dnsmasq is a low-resource DNS and DHCP server. Hostapd allows regular network interface cards to be treated as access points.

```console
sudo apt-get install dnsmasq hostapd
```

^Those software packages haven't been configured, so I don't want them to be running yet. Also, I'm rebooting just to keep things fresh.

```console
sudo systemctl stop dnsmasq
sudo systemctl stop hostapd
sudo reboot
```

IP addresses change on reboot *and* the wireless access point needs to be at the same IP address (so devices can stay on it), so I need to give my Pi a static IP address. I'll be doing that by making a dhcpcd configuration file. Nano is a common text editor, BTW.

```console
# This opens *or* creates the configuration file
sudo nano /etc/dhcpcd.conf
# this is the config info that I added to the end of the file
interface wlan0
    static ip_address=192.168.4.1/24
    nohook wpa_supplicant
```

The changes won't take effect until the service is restarted, so ...

```console
sudo service dhcpcd restart
```

I'm about to make changes to the dnsmas configuration file, so I'll be saving an original copy just in case ;)

```console
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig  
sudo nano /etc/dnsmasq.conf
```

Here's the new code for the dnsmasq config file:

```console
interface=wlan0      # Use the require wireless interface - usually wlan0
  dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
```

^It's now set up to provide IP addresses between 192.168.4.2 and 192.168.4.20 AND the IP addresses will only be good for 24 hours.

But how do I set up the Wi-Fi name, password & GHz info?!? With another configuration file! Here I'm adding more specifications for the hostapd configuration file. Please don't use my password to hax my network, lol ;)

```console
interface=wlan0
driver=nl80211
ssid=NameOfNetwork
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

The current DAEMON_CONF is set to a blank string, so the Pi doesn't know where the hostapd file is. This new addition tells my Raspberry where the hostapd configuration file is:

```console
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

**This is where the tutorial fails me!** I get an error about not being able to start the hostapd service because it is "masked."

```console
pi@raspberrypi:~ $ sudo systemctl start hostapd
Failed to start hostapd.service: Unit hostapd.service is masked.
pi@raspberrypi:~ $ sudo systemctl start dnsmasq
```

Thankfully, I know how to Google around ;) Another user experienced the same problem, [explaining it here.](https://github.com/raspberrypi/documentation/issues/1018) I ran their commands and that set things right :)

```console
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
sudo systemctl start hostapd
```

Now I add the masquerade algorithm for outbound traffic on eth0 (eth0 is ethernet 0 in UNIX-based systems).

```console
sudo iptables -t nat -A  POSTROUTING -o eth0 -j MASQUERADE
```

This is me saving the iptables rule.

```console
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

Now I edit the /etc/rc.local file so that the previously added rules run after a reboot.

```console
pi@raspberrypi:~ $ sudo nano /etc/rc.local
# add this line above the "exit 0"
iptables-restore < /etc/iptables.ipv4.nat
# reboot the system
pi@raspberrypi:~ $ sudo reboot
```

I should now be seeing a net Wi-Fi access point ... YES! I did a search on my phone and, there it is!!!

![New_WiFi_Name](/assets/2019-04-04-NAT_Raspberry-WAP/New_WIFI_Name.png)

^BUT, that network doesn't have access to the internet because I haven't yet *bridged* the Raspberry Pi's ethernet connection to it's new Wi-Fi access point. I'll need the bridge-utils to make that happen ...

```console
sudo apt-get install bridge-utils
```

I'm gonna turn off the host access point until it's properly configured.

```console
sudo systemctl stop hostapd
```

I then added these two lines to the end of the /etc/dhcpdd config file, *BUT* the lines were added above the previously added interface lines.

```console
# I used "sudo nano /etc/dhcpcd.conf" to open the file
# then I added these lines
denyinterfaces wlan0
denyinterfaces eth0
```

Here's  how I added a bridge named br0:

```console
sudo brctl addbr br0
```

This line connects the eth0 and the br0 network ports:

```console
sudo brctl addif br0 eth0
```

Next up, I needed to make some edits to make the interfaces work with bridging. I opened the interfaces file with:

```console
sudo nano /etc/network/interfaces
```

These are the changes that I made to the end of that file:

```console
# Bridge setup
auto br0
iface br0 inet manual
bridge_ports eth0 wlan0
```

Finally, I configured the hostapd configuration file again to include this new br0 connection ... this means that devices connecting to the WAP will have access to the bridged ethernet internet connection! Here's how I opened the configuration file:

```console
pi@raspberrypi:~ $ sudo nano /etc/hostapd/hostapd.conf
```

These are the lines that I edited ... note that I have changed the SSID to be "DavidWiFiTest" ... I wanna try out underscores, but I don't know if those are allowed ... it's 1:37 AM, so that can wait, lol.

```console
interface=wlan0
bridge=br0
#driver=nl80211
ssid=DavidWiFiTest
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

^All of the edits are now in place, so it's time for a final reboot to run the relevant changes.

```console
sudo reboot
```

annnnnnnnndddddd .... YESSSSSSS! My new Wi-Fi access point is available AND, this time, it now gives attached devices access to it's internet connection!!!

![](/assets/2019-04-04-NAT_Raspberry-WAP/Dave_WiFi.png)
