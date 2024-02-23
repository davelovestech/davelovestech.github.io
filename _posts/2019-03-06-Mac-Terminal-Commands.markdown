---
layout: post
title:  "Mac Terminal Commands"
date:   2019-03-06 11:00:40 -0700
categories: macOS
---

In this blog post I will follow a Mac Terminal commands on the Mac Mojave operating system. More specifically, I will be following the tutorial called [HOW TO USE BASIC UNIX COMMANDS TO WORK IN TERMINAL ON YOUR MAC](https://www.dummies.com/computers/macs/mac-operating-systems/how-to-use-basic-unix-commands-to-work-in-terminal-on-your-mac/). I won't spend too much time on this because Mac is mostly identical to Linux, which is the main OS than I use. I have posted the terminal commands I entered and the output for you to examine.

# What Version of Mac do I Have?
You can use the sw_vers command to get information about the current Mac operating system.
```console
davids-iMac:~ david$ sw_vers
ProductName:	Mac OS X
ProductVersion	10.14
BuildVersion:	18A391
```

# UNIX Directory Commands
These are commands for working with directories in a Unix system. Mac and Linux are both Unix-like systems. As far as I can tell, all of the commands I use in this article would work the same in my Ubuntu Linux system.
### What Directory am I In?
The print working directory, pwd, command will return where you currently are in the system.
```console
davids-iMac:~ david$ pwd
/Users/david
```
### List Directory Contents
The list command will return the contents of the current directory. \
```console
davids-iMac:~ david$ ls
Desktop		Downloads	Movies		Pictures
Documents	Library		Music		Public
```
### Creating a New Directory
You can use the change directory, cd, command to switch directories.
```console
davids-iMac:~ david$ cd Documents
davids-iMac:Documents david$ ls
another_file.txt		i_can_make_files.EXTENSION
davids-iMac:Documents david$ cd ../
davids-iMac:~ david$
```
### Making a Directory
You can use the mkdir command to create a new directory.
```console
davids-iMac:~ david$ ls
Desktop		Downloads	Movies		Pictures
Documents	Library		Music		Public
davids-iMac:~ david$ mkdir EXAMPLE_DIRECTORY
davids-iMac:~ david$ ls
Desktop		EXAMPLE_DIRECTORY	Music
Documents	Library			Pictures			
Downloads	Movies			Public
```
### Deleting a Directory
You can use the rmdir command to remove a directory.
```console
davids-iMac:~ david$ ls
Desktop		EXAMPLE_DIRECTORY	Music
Documents	Library			Pictures			
Downloads	Movies			Public
davids-iMac:~ david$ rmdir EXAMPLE_DIRECTORY
davids-iMac:~ david$ ls
Desktop		Downloads	Movies		Pictures
Documents	Library		Music		Public
```

# Working with Files
### Creating Files
You can use the touch command to create a new file.
```console
davids-iMac:Documents david$ touch i_can_make_files.EXTENSION
davids-iMac:Documents david$ ls
i_can_make_files.EXTENSION
```
### Copying Files
The command for copying is cp.
```console
davids-iMac:Documents david$ ls
i_can_make_files.EXTENSION
davids-iMac:Documents david$ cp i_can_make_files.EXTENSION COPY-OF-I-CAN-MAKE-FILES.EXTENSION
davids-iMac:Documents david$ ls
i_can_make_files.EXTENSION	COPY-OF-I-CAN-MAKE-FILES.EXTENSION
```
### Moving Files
mv filename1 filename2 this moves a file or changes its name
```console
davids-iMac:Documents david$ mv COPY-OF-I-CAN-MAKE-FILES.EXTENSION ../
davids-iMac:Documents david$ ls
i_can_make_files.EXTENSION
davids-iMac:Documents david$ cd ../
davids-iMac:Documents david$ ls
COPY-OF-I-CAN-MAKE-FILES.EXTENSION
Desktop
Documents
Downloads
Library
Movies
Music
Pictures
Public
```
### Removing Files
You can use the rm command to remove a file.
```console
davids-iMac:Documents david$ ls
COPY-OF-I-CAN-MAKE-FILES.EXTENSION
Desktop
Documents
Downloads
Library
Movies
Music
Pictures
Public
davids-iMac:Documents david$ rm COPY-OF-I-CAN-MAKE-FILES.EXTENSION
davids-iMac:Documents david$ ls
Desktop
Documents
Downloads
Library
Movies
Music
Pictures
Public
```
