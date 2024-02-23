---
layout: post
title:  "Making a Print Server with SMB (SAMBA)"
date:   2019-04-03 1:00:40 -0700
categories: Networking
---

I have a Brother Monochrome Laser Printer Model # HL-L2320D. It's always had a USB line to my Ryzen 3 Windows 10 machine and that's been fine for me. I have systems running Windows, macOS, and Linux ... but most of the office-type work that I do involves Windows, so there isn't really a reason to connect to my other machines ... except for the challenge of building a print server!

I recently purchased a Raspberry Pi and I've been blown away by all of the cool projects I've been able to do with it. In this post I'll guide you through how I used the SAMBA implementation of the Server Message Block (SMB) protocol to make a print server. I followed [a tutorial on print servers from PiMyLifeUp.](https://pimylifeup.com/raspberry-pi-print-server/)

As usual, the first thing that I did was make sure that my Pi was completely up to date.

```console
sudo apt-get update
sudo apt-get upgrade
```

I'll be using the Common UNIX Printing System (CUPS) as a server, so that package is required:

```console
sudo apt-get install cups
```

The pi user is not part of the default admin group, so I need to run this command.

```console
pi@raspberrypi:~ $ sudo usermod -a -G lpadmin pi
```

Additionally, the default is not to have CUPS accessible to the whole network. I have other systems that I'd like to print from, so I will be allowing remote connections from any system on my network.

```console
pi@raspberrypi:~ $ sudo cupsctl --remote-any
pi@raspberrypi:~ $ sudo /etc/init.d/cups restart
[ ok ] Restarting cups (via systemctl): cups.service.
pi@raspberrypi:~ $
```

This is bad from a security perspective ... I'm starting to understand why people use Raspberry Pi's as standalone devices (and also the importance of a firewall).

```console
pi@raspberrypi:~ $ hostname -I
192.168.1.3 192.168.1.4
```

CUPS runs on port 631, so the admin interface should be here: http://192.168.1.3:631/

![CUPS-631](/assets/2019-04-05-Print_Server_Raspberry/CUPS-631.PNG)

^The print server is running! I want to be able to print from Windows, so I will need to set up the SAMBA implementation of SMB.

```console
pi@raspberrypi:~ $ sudo apt-get install samba
```

Next up is to make some edits to the samba configuration file. This is how I opened the file for editing:

```console
pi@raspberrypi:~ $ sudo nano /etc/samba/smb.conf
```

Here's how I changed the config file. Most of this was already there ... I just needed to switch around some "yes" and "no" conditions.

```console
# CUPS printing.  
[printers]
comment = All Printers
browseable = no
path = /var/spool/samba
printable = yes
guest ok = yes
read only = yes
create mask = 0700

# Windows clients look for this share name as a source of downloadable
# printer drivers
[print$]
comment = Printer Drivers
path = /var/lib/samba/printers
browseable = yes
read only = no
guest ok = no
```

^That configuration won't run until the service is restarted.

```console
pi@raspberrypi:~ $ sudo /etc/init.d/samba restart
[ ok ] Restarting nmbd (via systemctl): nmbd.service.
[ ok ] Restarting smbd (via systemctl): smbd.service.
pi@raspberrypi:~ $
```

Now it's time to add my printer! I headed back over to https://http://192.168.1.3:631/

There's gonna be a lot of clicking and I don't wanna get picutres of all of it. If you wanna replicate this, go to the tutorial at [PiMyLifeUp.](https://pimylifeup.com/raspberry-pi-print-server/)

1. Click "Administration" from the top menu
2. Click the "Add Printer" button from the Printers menu
3. Select your printer from the "Local Printers" list
  * Mine is Brother HL-L2320D series (Brother HL-L2320D series)
4. Click "Continue"
5. Select "Share This Printer" to allow other systems access
6. Select "Continue"

Hopefully your printer driver will be part of the driver list ... unfortunately my printer isn't here :( Digging a little, I see that my printer is listed in the [Open Printing Database](https://www.openprinting.org/printer/Brother/Brother-HL-L2320D) ... *however*, it doesn't work on a Raspberry Pi with the normal approach. Luckily, I understand how to use Google! Apparently most Brother printers support standard printer languages, BUT the laser printers commonly don't ... The good news is that I found a [GitHub repo for a DIY printer driver](https://github.com/pdewacht/brlaser) for my situation :D

Here's the default list of drivers ... note the initial lack of brlaser drivers:

![no-brlaser-drivers](/assets/2019-04-05-Print_Server_Raspberry/no-brlaser-drivers.PNG)

The GitHub repo mentions that Raspbian ships with this driver ... so I gave this command a try:

```console
pi@raspberrypi:~ $ sudo apt-get install printer-driver-brlaser
```

Awesomeness! I've got three brlaser drivers ... they're not specifically for my printer, BUT the GitHub repo makes the claim that my Brother HL-L2320D printer is supported by the brlaser drivers ... sooooooo, I'm gonna guess that the developer just hasn't updated naming conventions. What happens when I pick the "Brother DCP-7030, using brlaser v3 (en)" driver? Will it work?!? I clicked "Add Printer." Then I selected "Set Default Options."

I headed over to my Network connections and I still don't see my printer. I'll try rebooting the Raspberry Pi.

```console
pi@raspberrypi:~ $ sudo reboot
Connection to 192.168.1.3 closed by remote host.
Connection to 192.168.1.3 closed.
```

My Raspberry Pi print server is now recognized!

![brlaser-drivers-are-here](/assets/2019-04-05-Print_Server_Raspberry/brlaser-drivers-are-here.PNG)

I double-clicked my printer and was alerted that there was "No driver found." Weirdly enough, Windows then selected a list of printers --with my printer-- and selecting my printer still ended up with an inability to connect to the printer. I tried selecting Windows Update and it had my driver all the way at the bottom. I selected that and then I oddly ended up with two potential printers ...

* Brother_HL-L2320D_series on RASPBERRYPI
* Brother HL-L2320D series @ raspberrypi

I don't know why there are two. I also don't know why the one with lower-case "raspberrypi" does not work, BUT the wone with upper-case "RASPBERRYPI" DOES WORK to print on Windows, so I'm happy!
