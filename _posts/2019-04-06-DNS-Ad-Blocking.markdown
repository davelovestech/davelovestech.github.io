---
layout: post
title:  "Network-Wide DNS-Level Ad Blocking"
date:   2019-04-06 6:30:40 -0700
categories: Networking
---

I don't wanna see ads, so I have been using AdBlock to stop them at a browser level. There are some problems with this method:

* My bandwith is still used to download the ads
* I have to install AdBlock on every system
* it requires processing power

I have a Raspberry Pi now! This means that I can use it to run DNS-level ad blocking for my *entire* network! What does that mean?

My website isn't located at www.davehalvorsen.tech ... it has an IP address, which is kinda like a phone number. Here's how I got the IP address of my website:

```console
pi@raspberrypi:/media/1TB_Toshiba $ host www.davehalvorsen.tech
www.davehalvorsen.tech is an alias for davehalvorsen.github.io.
davehalvorsen.github.io has address 185.199.111.153
davehalvorsen.github.io has address 185.199.110.153
davehalvorsen.github.io has address 185.199.109.153
davehalvorsen.github.io has address 185.199.108.153
```

^Ha! I forgot that I was still SSHd into my Raspberry Pi when I wrote that. It'd work the same in Ubuntu. Here's how to do the equivalent of that in Windows.

```console
C:\Users\dave>ping www.davehalvorsen.tech

Pinging davehalvorsen.github.io [185.199.108.153] with 32 bytes of data:
Reply from 185.199.108.153: bytes=32 time=5ms TTL=56
Reply from 185.199.108.153: bytes=32 time=9ms TTL=56
Reply from 185.199.108.153: bytes=32 time=14ms TTL=56
Reply from 185.199.108.153: bytes=32 time=14ms TTL=56

Ping statistics for 185.199.108.153:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 5ms, Maximum = 14ms, Average = 10ms
```

Computers would prefer to deal with the IP address and humans would prefer to remember the simplified name, so DNS works as a bridge between the two.


[Pi-hole](https://pi-hole.net/) acts as a local DNS for your network and it blacklists advertisements from ever being downloaded! It is only as good as the rules that it's programmed to follow, so it doesn't block everything ... for example, YouTube ads still run with the Pi-hole defaults. However, this means that you can play with new rules to try and block out YouTube ads! :)

The fast route to getting Pi-hole on your Raspberry Pi is to run this command:

```console
curl -sSL https://install.pi-hole.net | bash
```

^HOWEVER, it's never a good idea to pipe unknown commands directly into your bash! Think of the security implications :S It's important to be a bit more mindful!

I'll be downloading from the Git repository and running from there. There's not really a conceptual difference between piping directly into bash vs. downloading the Git repo and running from there *except* it forces you to do a bit more research about what you're about to install.

I haven't used Git on my Raspberry Pi yet, so I wanted to make sure it's installed.

```console
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Nov 13 06:24:58 2018
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ ls
Desktop    Downloads  Music     Public     Videos
Documents  MagPi      Pictures  Templates
pi@raspberrypi:~ $ cd Downloads/
pi@raspberrypi:~/Downloads $ git --version
git version 2.11.0
```

Here's how I cloned the repo and installed it.

```console
pi@raspberrypi:~/Downloads $ git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
Cloning into 'Pi-hole'...
remote: Enumerating objects: 81, done.
remote: Counting objects: 100% (81/81), done.
remote: Compressing objects: 100% (73/73), done.
remote: Total 81 (delta 6), reused 30 (delta 0), pack-reused 0
Unpacking objects: 100% (81/81), done.
pi@raspberrypi:~/Downloads $ ls
Pi-hole
pi@raspberrypi:~/Downloads $ cd Pi-hole/
pi@raspberrypi:~/Downloads/Pi-hole $ ls
advanced           block hulu ads   LICENSE   README.md         test
automated install  CONTRIBUTING.md  manpages  requirements.txt  tox.ini
autotest           gravity.sh       pihole    setup.py
pi@raspberrypi:~/Downloads/Pi-hole $ cd automated\ install/
pi@raspberrypi:~/Downloads/Pi-hole/automated install $ ls
basic-install.sh  uninstall.sh
pi@raspberrypi:~/Downloads/Pi-hole/automated install $ sudo bash basic-install.sh
```

It's important that I clarify that I'll still be using a DNS out there on the internet. It's just that I'm going to be filtering that through my local DNS server. Here's the Pi-hole configuration page in which I selected Google as my upstream DNS provider:

![google-dns](/assets/2019-04-05-DNS_Raspberry/google-dns.PNG)

I need to tell my router to stop using the default DNS and start using my Pi as a local DNS. Here's how I did that from my router's config page:

![router-set-dns](/assets/2019-04-05-DNS_Raspberry/router-set-dns.PNG)

^Note that I'm still using Google's DNS as a backup. I'd rather have ads than lose internet :)

Check it out! Here's the admin page for Pi-hole:

![admin-page](/assets/2019-04-05-DNS_Raspberry/admin-page.PNG)

I'm going to be loading websites with & without Pi-hole enabled, so it's important to mention the DNS cache. Grabbing the DNS on frequently visited website *every* time would be wasteful from a resource perspective, so DNS results are cached. This is normally great, but it'll screw up the +/- Pi-hole testing, so I'll be clearing the DNS cache after each experiment. Here's how to do that on Windows:

```console
C:\Users\dave>ipconfig /flushdns

Windows IP Configuration

Successfully flushed the DNS Resolver Cache.
```

Alright, so here's Buzzfeed with Pi-hole turned off:

![ad-on-buzzfeed](/assets/2019-04-05-DNS_Raspberry/ad-on-buzzfeed.PNG)

And here's Buzzfeed with Pi-hole turned on:

![buzzfeed-blocked-ad](/assets/2019-04-05-DNS_Raspberry/buzzfeed-blocked-ad.PNG)

What about on my phone? Does it work there? YES! Here's Buzzfeed from my phone with Pi-hole turned off:

![buzzfeed-phone-ad](/assets/2019-04-05-DNS_Raspberry/buzzfeed-phone-ad.png)

Here's Buzzfeed from my phone with Pi-hole turned on:

![buzzfeed-phone-NO-ad](/assets/2019-04-05-DNS_Raspberry/buzzfeed-phone-NO-ad.png)

ISN'T THIS AWESOME?!? YouTube ads still run, but apparently it's possible to block those with some clever rules ... I'll be trying that eventually. However, I'm gonna stick with AdBlock & Pi-hole working together for the time being ;)
