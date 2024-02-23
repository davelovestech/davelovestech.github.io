---
layout: post
title:  "SSH into the Raspberry Pi"
date:   2019-04-01 11:00:40 -0700
categories: Windows Linux
---

The Raspberry Pi is a fun mini-computer that you can buy for around $35. HDMI is the only display output that mine has and it's been a bummer having to hook it up to a monitor just to enter a few commands. Surprisingly only one of my monitors has an HDMI port *and* it's the center 27-inch display, so it's been a bummer having to dedicate that to the Pi.

Apparently you can use VNC to get a visual display of the Raspberry Pi from another computer, but I don't need a GUI for my projects. Luckily, you can SSH into the Pi! SSH stands for the secure shell. Back in the day, folks would Telnet into a foreign system using port 23 and that's insecure because passwords are sent as plaintext. SSH uses cryptography to hide messages!

The Raspberry Pi has SSH disabled by default, so I needed to enable it. The path for the config menu is Menu > Preferences > Raspberry Pi Configuration. I selected "Enabled" for SSH, which means that I can now have my 27-inch monitor back! Here's that config menu:

![enable_ssh](/assets/2019-04-04-SSH-Raspberry/enable_ssh.jpg)

Linux and Mac have a default SSH capability, but Windows is a little behind. It needs an SSH client to work. PuTTY seems to be the main one, so that's what I use. PuTTY requires a port number and an IP address to start. The port is 22 because I'm doing SSH. The IP address is ... well, what is the IP address?!? It's 192.168.1.3 *or* 192.168.1.4. I could use ifconfig to get *tons* of output or just the numbers I'm interested in with the -I hostname argument. If you're wondering, the reason that I have two IP addresses is because my Pi has an ethernet and a wireless connection.

```console
pi@raspberrypi:~ $ hostname -I
192.168.1.3 192.168.1.4
```

Here's the PuTTY interface for Windows:

![enable_ssh](/assets/2019-04-04-SSH-Raspberry/PuTTY-login.PNG)

Here's how to SSH login from Ubuntu:

```console
dave@Ryzen3:~$ ssh pi@192.168.1.3
pi@192.168.1.3's password:
Linux raspberrypi 4.14.98-v7+ #1200 SMP Tue Feb 12 20:27:48 GMT 2019 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Apr  4 09:53:11 2019 from 192.168.1.2
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ ls
Desktop    Downloads  Music     Public     Videos
Documents  MagPi      Pictures  Templates
```

That's all I wanted to show in this article. Thankfully, I can have my 27-inch monitor back now :D
