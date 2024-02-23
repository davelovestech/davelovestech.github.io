---
layout: post
title:  "Adjusting the AD Group Policy for Firewall"
date:   2019-05-03 5:00:40 -0700
categories: Active_Directory 
---

In previous articles I have enabled Active Directory (AD) in Windows Server 2012 and added a computer and a user to that AD instance. But what good is Active Directory if you can't use it to push Group Policies to the relevant entities? In this post I'll show you how I enabled and disabled the Windows Firewall, for a client Windows 10 Enterprise OS, from the Windows Server 2012 Server instance.

You can make changes like that in the Group Policy Management Editor. They are called Group Policy Objects (GPO).

![select-windows-firewall-settings](/assets/2019-04-06-add-group-policy/select-windows-firewall-settings.PNG)

I went the route of Forst > Domains > Example.com > Lab to arrive at Computers. I right-clicked "Computers" and chose "Create a GPO in this domain, and Link it here..." I went with "Firewall Rules" for the new GPO.

I'm gonna do something very silly: disable the Windows Firewall. See how it's currently enabled in the Windows 10 Enterprise system?

![firewall-is-on](/assets/2019-04-06-add-group-policy/firewall-is-on.PNG)

Heading back into the Windows Server 2012 system, I opened up the Windows Firewall Properties by going this route: Computer Configuration > Policies > Windows Settings > Security Settings > Windows Firewall with Advanced Security. Windows Firewall Properties is under the Public Profile heading. I selected "Off" for the firewall state.

![turn-firewall-off](/assets/2019-04-06-add-group-policy/turn-firewall-off.PNG)

BUT, going back to the Windows 10 Enterprise system, the firewall is still on ... why?

![firewall-is-on](/assets/2019-04-06-add-group-policy/firewall-is-on.PNG)

It's because the Group Policy Object hasn't hit the Windows 10 Enterprise system yet. I opened up the command prompt and ran gpupdate /force to get the most recent Group Policy Object.

![gpupdate-force](/assets/2019-04-06-add-group-policy/gpupdate-force.PNG)

That's when the Windows 10 Enterprise Firewall turned off. To be clear, this is what's happened so far:

* I created a GPO for the firewall being off in Windows Server 2012
* I requested the most recent GPO from the server while logged in on the Windows 10 Enterprise system
* The Windows 10 Enterprise Firewall was turned off

![windows-firewall-off](/assets/2019-04-06-add-group-policy/windows-firewall-off.PNG)

The Group Policy change from the Windows Server 2012 system worked! That's what I wanted to cover in this post. I then went back into Windows Server 2012 and adjusted the firewall to be on.

![FINAL-windows-firewall-now-on](/assets/2019-04-06-add-group-policy/FINAL-windows-firewall-now-on.PNG)

I forced anther Group Policy update and found that the firewall was back on :)

![FINAL-windows-firewall-back-on](/assets/2019-04-06-add-group-policy/FINAL-windows-firewall-back-on.PNG)
