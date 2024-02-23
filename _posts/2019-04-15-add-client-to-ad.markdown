---
layout: post
title:  "Adding a User to a Windows AD Domain"
date:   2019-04-15 2:00:40 -0700
categories: Active_Directory
---

So far I've enabled DHCP & Active Directory on a Windows Server 2012 instance and I've also set up a pfSense Firewall for routing traffic. It's time to create a user account! The first thing to do is open up the Active Directory Administrative Center.

![active-directory-admin-center](/assets/2019-04-06-add-group-policy/active-directory-admin-center.PNG)

Organizational Units (OU) can receive Group Policy settings, so I created OU for "Computers" and "Users" *within* the OU of Lab. Here's the barebone setup for my domain:

![created-users-and-computers-for-lab](/assets/2019-04-06-add-group-policy/created-users-and-computers-for-lab.PNG)

It's possible to create a new user by selecting New > User. Here's the Jared user that I created. I added him to the "Domain Admins" group by going through the "Member Of" section.

![create-user-jared](/assets/2019-04-06-add-group-policy/create-user-jared.PNG)
