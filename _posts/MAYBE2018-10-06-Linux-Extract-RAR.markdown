---
layout: post
title:  "Extracting a RAR File in Linux"
date:   2018-10-06 22:42:40 -0700
categories: Linux
---

The unrar package can be used to open .rar files. Here's how to install unrar and the terminal output for the installation:

```console
[17:21] david@Ryzen VirtualBox VMs/ >  sudo apt-get install unrar
[sudo] password for david:
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  bsdtar cdbs dh-translations intltool libgstreamer-plugins-base0.10-0
  libgstreamer0.10-0 libllvm5.0 linux-headers-4.15.0-36
  linux-headers-4.15.0-36-generic linux-headers-4.15.0-39
  linux-headers-4.15.0-39-generic linux-image-4.15.0-36-generic
  linux-image-4.15.0-39-generic linux-modules-4.15.0-36-generic
  linux-modules-4.15.0-39-generic linux-modules-extra-4.15.0-36-generic
  linux-modules-extra-4.15.0-39-generic python-scour ruby-childprocess
  ruby-domain-name ruby-erubis ruby-ffi ruby-http-cookie ruby-i18n ruby-listen
  ruby-log4r ruby-mime-types ruby-net-scp ruby-net-sftp ruby-net-ssh
  ruby-netrc ruby-nokogiri ruby-rb-inotify ruby-rest-client ruby-sqlite3
  ruby-unf ruby-unf-ext sqlite3
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  unrar
0 upgraded, 1 newly installed, 0 to remove and 57 not upgraded.
Need to get 123 kB of archives.
After this operation, 310 kB of additional disk space will be used.
Get:1 http://us.archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 unrar amd64 1:5.3.2-1+deb9u1build0.16.04.1 [123 kB]
Fetched 123 kB in 1s (119 kB/s)
Selecting previously unselected package unrar.
(Reading database ... 494700 files and directories currently installed.)
Preparing to unpack .../unrar_1%3a5.3.2-1+deb9u1build0.16.04.1_amd64.deb ...
Unpacking unrar (1:5.3.2-1+deb9u1build0.16.04.1) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up unrar (1:5.3.2-1+deb9u1build0.16.04.1) ...
update-alternatives: using /usr/bin/unrar-nonfree to provide /usr/bin/unrar (unrar) in auto mode
```

The example file that I will use is "macOS Mojave HFS by Techsviewer.rar". I used the "unrar" command with the "e" option to extract the .rar file:

```console
[17:23] david@Ryzen VirtualBox VMs/ >  unrar e macOS\ Mojave\ HFS\ by\ Techsviewer.rar

UNRAR 5.30 beta 2 freeware      Copyright (c) 1993-2015 Alexander Roshal


Extracting from macOS Mojave HFS by Techsviewer.rar

Extracting  macOS Mojave HFS by Techsviewer.vmdk                      59%
```
