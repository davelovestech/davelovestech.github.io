---
layout: post
title:  "Linux Terminal Commands"
date:   2019-03-05 17:20:40 -0700
categories: Linux
---

I am currently preparing to take the A+ Exam and the Linux operating system is part of the tested material. Ubuntu Linux is an operating system that I have been using on and off for several years. The following is an overview of Linux commands and the resultant terminal output.

# What Version of Linux am I Running?
I am running Ubuntu 16.04. The current version of 
```console
$ cat /etc/os-release
NAME="Ubuntu"
VERSION="16.04.6 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.6 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```

# Making Directories and Moving Between Them
These commands are for obtaining location information and changing location.
## Print Working Directory
The "pwd" command prints the current working directory location. In this case, I am in the home directory for my user account. 
```console
$  pwd
/home/david
```
## List Current Directory
The way to list the contents of a directory is an "ls" command.
```console
$ ls
2018_David_Finance_Budget.xlsx_0.ods  Public           anaconda3
B%20ANSWER.odt_1.odt                  R                examples.desktop
Desktop                               Templates        exercise_B_notes_BRAIN.odt_0.odt
Documents                             Untitled.ipynb   exercise_D_notes.odt_1.odt
Downloads                             Untitled1.ipynb  gems
Dropbox                               Untitled2.ipynb  qa
Music                                 Videos           snap
Pictures                              VirtualBox VMs   sudo
```
## Changing Directories and Making Files and Folders
The way to change directories is a "cd" command. "cd ../" goes up one folder in the directory tree. "cd [DESTINATION_DIRECTORY_TREE]" will go to the requested location. In this example, I start in the home folder, go up one folder and then I change directories all the way down into the david folder. I use the "ls" command to show what is in each directory. 
```console
$ pwd
/home
$ cd ../
$ ls
bin    david  home            lib    lost+found  opt   run   srv  usr      vmlinuz.old
boot   dev    initrd.img      lib32  media       proc  sbin  sys  var
cdrom  etc    initrd.img.old  lib64  mnt         root  snap  tmp  vmlinuz
$ cd /home/david
$ ls
2018_David_Finance_Budget.xlsx_0.ods  Public           anaconda3
B%20ANSWER.odt_1.odt                  R                examples.desktop
Desktop                               Templates        exercise_B_notes_BRAIN.odt_0.odt
Documents                             Untitled.ipynb   exercise_D_notes.odt_1.odt
Downloads                             Untitled1.ipynb  gems
Dropbox                               Untitled2.ipynb  qa
Music                                 Videos           snap
Pictures                              VirtualBox VMs   sudo
``` 
## Make a Directory
You can make a new directory with the "mkdir" command. In this example, I create the directory "Test" and then I enter it with the cd command. I use "pwd" to see where I am in the directory tree as well.
```console
$ cd Linux_Practice/
$ ls
$ pwd
/media/david/Linux/Linux_Practice
$ mkdir Test
$ cd Test
$ pwd
/media/david/Linux/Linux_Practice/Test
$ ls
```

## Create a File
Hopefully you noticed that there was nothing in the /media/david/Linux/Linux_Practice/Test directory. You can create a file with the touch command.

```console
$ touch this-is-a-file
$ ls
Test  this-is-a-file
```
# Removing Directories and Moving Files
In this section I will review how to copy & move files as well as how to remove files or directories.

## Copy
You can copy a file with the "cp" command.
```console
$ cp this-is-a-file COPY-of-this-is-a-file
$ ls
COPY-of-this-is-a-file  Test  this-is-a-file
```
## Move
You can move a file with the mv command. Here I use the mkdir command to create a new directory and then the mv command to move the COPY-of-this-is-a-file into the new directory.
```console
$ ls
COPY-of-this-is-a-file  Test  new_directory  this-is-a-file
$ mv COPY-of-this-is-a-file ./new_directory/
$ ls
Test  new_directory  this-is-a-file
$ cd new_directory/
$ ls
COPY-of-this-is-a-file
```

## Remove
You can remove a file or folder with the "rm" command. First I remove the "this-is-a-file" file and then I remove the "new_directory". Take note of the error! I needed to add that the -r flag is for "recursive" and that the -f flag is for "force."
```console
$ ls
Test  new_directory  this-is-a-file
$ rm this-is-a-file 
$ ls
Test  new_directory

$ rm new_directory/
rm: cannot remove 'new_directory/': Is a directory
$ rm -rf new_directory/
$ ls
Test
``` 

# Changing File Mode and Ownership
These commands are for changing the permissions of a file. 
## Changing File Mode
I'm going to use a the vi editor and a bit of the Python programming language here for example purposes. Don't worry about knowing Python or the vi editor. Pay attention to
```console
$ vi example_python.py
$ ls
Test  example_python.py  python_print_example.py  python_test.py
$ ./example_python
bash: ./example_python: No such file or directory
$ chmod u+x example_python.py 
$ ./example_python.py 
This is a test!
$ cat example_python.py 
#!/usr/bin/env python
print("This is a test!")
```
## Changing File Owner
As you can see in the first "ls -alt" command, the example_python.py file is owned by the user "david":
```console
[22:44] david@Ryzen Linux_Practice/ >  ls -alt
total 24
drwxrwxr-x  3 david david 4096 Mar  5 22:43 .
-rwxrw-r--  1 david david   48 Mar  5 22:43 example_python.py
-rwxrw-r--  1 david david   48 Mar  5 22:42 python_test.py
-rwxrw-r--  1 david david   49 Mar  5 22:41 python_print_example.py
drwxrwxr-x  2 david david 4096 Mar  5 22:12 Test
drwx------ 40 david david 4096 Mar  5 22:11 ..
```
I use the chown command to change the owner to the "mysql" user:
```console
$ sudo chown mysql example_python.py 
$ ls -alt
total 24
drwxrwxr-x  3 david david 4096 Mar  5 22:43 .
-rwxrw-r--  1 mysql david   48 Mar  5 22:43 example_python.py
-rwxrw-r--  1 david david   48 Mar  5 22:42 python_test.py
-rwxrw-r--  1 david david   49 Mar  5 22:41 python_print_example.py
drwxrwxr-x  2 david david 4096 Mar  5 22:12 Test
drwx------ 40 david david 4096 Mar  5 22:11 ..
```

# Listing and Ending Processes
These are some commands for printing a list of processes and for ending a process.
## Print Task List
The process status "ps" command will list all of the processes. Here I used the -aux to obtain all of the processes that are currently running on the system. The list is huge, so I limited it to the top 10 rows with the pipe to head command " | head". 
```console
$ ps -aux | head
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 185692  6384 ?        Ss   10:23   0:01 /sbin/init splash
root         2  0.0  0.0      0     0 ?        S    10:23   0:00 [kthreadd]
root         4  0.0  0.0      0     0 ?        I<   10:23   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        I<   10:23   0:00 [mm_percpu_wq]
root         7  0.0  0.0      0     0 ?        S    10:23   0:00 [ksoftirqd/0]
root         8  0.0  0.0      0     0 ?        I    10:23   0:16 [rcu_sched]
root         9  0.0  0.0      0     0 ?        I    10:23   0:00 [rcu_bh]
root        10  0.0  0.0      0     0 ?        S    10:23   0:00 [migration/0]
root        11  0.0  0.0      0     0 ?        S    10:23   0:00 [watchdog/0]
```
## Kill a Task
You can end a task with the kill command. You need to know its PID, which is its process identifier. You can see the PID column in the previous section. However, that list is incredibly long. You can use pgrep to get the PID of a specific application. In this case, I will use pgrep to check for a Firefox instance *before* I launch it, and then get the PID after I launch it. Finally I will end the task with the "kill" command.
```console
$ pgrep firefox
$ firefox
$ pgrep firefox
9119
$ kill 9119
```
# Networking Commands
These commands are used for troubleshooting Linux Networking issues. 
## Getting Wired Network Information
The "ifconfig" command will return the IP address information for the wired internet connection. I removed a variety of outputs because I did not want to share personal information. HWaddr is the MAC address. inet addr is the IP address of the current interface. Bcast is the broadcast address. Mask is 255.255.255.0 because my IP address is a class C IP address. inet6 addr is the IPv6 address.   

```console
ifconfig
enp9s0    Link encap:Ethernet  HWaddr REDACTED
          inet addr:REDACTED  Bcast:REDACTED  Mask:255.255.255.0
          inet6 addr: REDACTED Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:992732 errors:0 dropped:0 overruns:0 frame:0
          TX packets:306354 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1274820372 (1.2 GB)  TX bytes:49320495 (49.3 MB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12360 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12360 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:6357101 (6.3 MB)  TX bytes:6357101 (6.3 MB)
```

## Getting Wireless Network Information
The "iwconfig" command will get wireless network information. I'm writing this post from a desktop computer, so it is no surprise that there aren't any wireless extensions.
```console
iwconfig
enp9s0    no wireless extensions.

lo        no wireless extensions.
```
## traceroute 
Traceroute can be used to determine how packets are sent to a destination. In the following case I use it to determine how my packets are sent to the Google DNS at 8.8.8.8. Note that I have redacted the first 6 lines of output because I do not wish to share the packet path for my home network. I'm still learning about network security, so I redacted everything except for the final DNS location. I imagine I could shared more, but I wanted to be safe.  
```console
traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  REDACTED
 2  REDACTED
 3  REDACTED
 4  REDACTED
 5  REDACTED
 6  REDACTED
 7  REDACTED
 8  REDACTED
 9  REDACTED
10  google-public-dns-a.google.com (8.8.8.8)  40.299 ms  40.274 ms  40.284 ms
``` 
## Ping an IP Address
You can use ping to determine if you have internet connectivity to a specific IP address. The -c flag is used to specify a number of times to ping the destination. In the following case, I pinged my own computer at the address of 127.0.0.1; this is always the home address for whatever system you are working on. 
```console
ping 127.0.0.1 -c 4
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.034 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.041 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.047 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.042 ms

--- 127.0.0.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3052ms
rtt min/avg/max/mdev = 0.034/0.041/0.047/0.004 ms
```

# Shut Down and Restart
I hope you enjoyed that walk through Linux commands! Now that we're done, I will show you how to shutdown, restart, and cancel a shutdown or restart. 
## Shutting Down the Computer
I used the h argument here to shutdown the computer. h stands for halt. 
```console
shutdown -h +3
Shutdown scheduled for Tue 2019-03-05 23:41:05 PST, use 'shutdown -c' to cancel.
```
## Restarting the Computer
You can restart a computer instead of shutting it down with the -r argument. r stands for restart.
```console
shutdown +3 -r
Shutdown scheduled for Tue 2019-03-05 23:42:48 PST, use 'shutdown -c' to cancel.
```
## Canceling a Shutdown or Restart
You can cancel a shutdown with the -c argument. c is for cancel.
```console
[23:38] david@Ryzen ~/ >  shutdown -c
```

Goodbye!

