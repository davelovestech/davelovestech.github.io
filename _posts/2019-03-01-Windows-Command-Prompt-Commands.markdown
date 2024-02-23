---
layout: post
title:  "Windows Command Prompt Commands"
date:   2019-03-01 22:20:40 -0700
categories:
---

I was lucky enough to have access to a Windows 95 computer back when I was a kid. I remember loving to watch the Disk Defragmenter graphical user interface way back then! I currently run a dual-boot desktop computer system with Windows 10 and Ubuntu 16.04. I'm currently writing this blog post in the Linux boot system. However, I have a Windows 10 VirtualBox instance that I will be running commands within. 

# What Version of Windows is This?
The systeminfo command will yield a long list of information about the computer system. It is possible to specifically filter to OS Name and Version with the findstr.
```console
C:\Users\IEUser>systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
OS Name:			Microsoft Windows 7 Enterprise
OS Version:			6.1.7601 Service Pack 1 Buld 7601
```

# Making Directories and Moving Between Them
The default in Windows command prompt is to print the working directory on each terminal line. It looks like this:
```console
C:\Users\IEUser>
```
 
### Changing Current Directory
I want to print the contents of a directory, but the IEUser directory is HUGE! I will use the change directory command "cd" to move one directory up.
```console
C:\Users\IEUser>cd ../
C:\Users>
```
### Listing Directory Contents
Here I used the directory command "dir" to list the contents of the current directory.
```console
C:\Users>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users

04/25/2018  07:59 AM    <DIR>          .
04/25/2018  07:59 AM    <DIR>          ..
03/02/2019  02:25 PM    <DIR>          IEUser
04/25/2018  07:48 AM    <DIR>          Public
03/02/2019  02:22 PM    <DIR>          sshd_server
               0 File(s)              0 bytes
               5 Dir(s)  28,680,577,024 bytes free
```

### Make a Directory
Here I made a directory called TEST, cd into, make a directory called TEST_DIRECTORY, and then I use DIR to list the directories again. Notice that there is now a new directory!
```console
C:\Users\IEUser>md TEST

C:\Users\IEUser>cd TEST

C:\Users\IEUser\TEST>DIR
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:45 AM    <DIR>          .
03/06/2019  12:45 AM    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  24,924,737,536 bytes free

C:\Users\IEUser\TEST>md TEST_DIRECTORY

C:\Users\IEUser\TEST>DIR
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:45 AM    <DIR>          .
03/06/2019  12:45 AM    <DIR>          ..
03/06/2019  12:45 AM    <DIR>          TEST_DIRECTORY
               0 File(s)              0 bytes
               3 Dir(s)  24,916,852,736 bytes free

C:\Users\IEUser\TEST>
```
### Delete a Directory
The rd command is used to remove a directory. Here I used dir to show the presence of BAD_DIRECTORY and then I use rd to get rid of it. DIR is used to show that the directory is actually gone.
```console
C:\Users\IEUser\TEST>md BAD_DIRECTORY

C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  01:10 AM    <DIR>          .
03/06/2019  01:10 AM    <DIR>          ..
03/06/2019  01:10 AM    <DIR>          BAD_DIRECTORY
               0 File(s)              0 bytes
               3 Dir(s)  24,472,600,576 bytes free

C:\Users\IEUser\TEST>rd /s /q BAD_DIRECTORY

C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  01:11 AM    <DIR>          .
03/06/2019  01:11 AM    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  24,472,600,576 bytes free
```

# Creating, Viewing, Moving, & Deleting Files
### Creating & Viewing File Contents
In this section I first use dir to show that there are no initial files. I then use echo and > to insert the text "this line is going into a file" into a file called THIS_IS_A_FILE.TXT.

```console
C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:54 AM    <DIR>          .
03/06/2019  12:54 AM    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  24,473,047,040 bytes free

C:\Users\IEUser\TEST>echo this line is going into a file > THIS_IS_A_FILE.TXT

C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:55 AM    <DIR>          .
03/06/2019  12:55 AM    <DIR>          ..
03/06/2019  12:55 AM                33 THIS_IS_A_FILE.TXT
               1 File(s)             33 bytes
               2 Dir(s)  24,472,911,872 bytes free

C:\Users\IEUser\TEST>type THIS_IS_A_FILE.TXT
this line is going into a file

C:\Users\IEUser\TEST>
```
### Viewing the Text of a File
Here I use the type command to view the contents of a file.
```console

C:\Users\IEUser\TEST>echo these are words in a file > FILENAME.txt

C:\Users\IEUser\TEST>type FILENAME.txt
these are words in a file
```
### Deleting a File
Here I use dir to show that THIS_IS_A_FILE.TXT is initially present. I then use the delete command, del, to remove the file. I then use dir one more time to show that the file is gone.
```console
C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:55 AM    <DIR>          .
03/06/2019  12:55 AM    <DIR>          ..
03/06/2019  12:55 AM                33 THIS_IS_A_FILE.TXT
               1 File(s)             33 bytes
               2 Dir(s)  24,472,289,280 bytes free

C:\Users\IEUser\TEST>del THIS_IS_A_FILE.TXT

C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  12:58 AM    <DIR>          .
03/06/2019  12:58 AM    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  24,472,289,280 bytes free
```
### Moving a File in a Directory
Here I created a file called FILENAME.txt with echo and >. Then I used move to place that file into DIRECTORY_ONE. I finally cd into DIRECTORY_ONE and use DIR to show that the file has indeed moved.
```console
C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  01:02 AM    <DIR>          .
03/06/2019  01:02 AM    <DIR>          ..
03/06/2019  01:02 AM    <DIR>          DIRECTORY_ONE
03/06/2019  01:02 AM    <DIR>          DIRECTORY_TWO
03/06/2019  01:02 AM                29 FILENAME.txt
               1 File(s)             29 bytes
               4 Dir(s)  24,475,189,248 bytes free

C:\Users\IEUser\TEST>move FILENAME.txt c:\Users\IEUser\TEST\DIRECTORY_ONE
        1 file(s) moved.

C:\Users\IEUser\TEST>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST

03/06/2019  01:03 AM    <DIR>          .
03/06/2019  01:03 AM    <DIR>          ..
03/06/2019  01:03 AM    <DIR>          DIRECTORY_ONE
03/06/2019  01:02 AM    <DIR>          DIRECTORY_TWO
               0 File(s)              0 bytes
               4 Dir(s)  24,475,189,248 bytes free

C:\Users\IEUser\TEST>cd DIRECTORY_ONE

C:\Users\IEUser\TEST\DIRECTORY_ONE>dir
 Volume in drive C has no label.
 Volume Serial Number is 3A97-874F

 Directory of C:\Users\IEUser\TEST\DIRECTORY_ONE

03/06/2019  01:03 AM    <DIR>          .
03/06/2019  01:03 AM    <DIR>          ..
03/06/2019  01:02 AM                29 FILENAME.txt
               1 File(s)             29 bytes
               2 Dir(s)  24,475,189,248 bytes free
```


# Listing and Ending Tasks
This is how to list current processes and kill selected processes. 
### List Current Processes
tasklist is a command for getting all of the current processes. There are a lot of processes, so I used "| more" to get the output as blocks of text. I also manually cut the number of rows to a short number. I'm not yet sure how to use the Windows equivalent of the Linux HEAD command, so this will have to do. 
```console
C:\Users\IEUser\TEST>tasklist | more

Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          8 K
System                           4 Services                   0        148 K
Registry                        88 Services                   0      7,868 K
smss.exe                       308 Services                   0      1,028 K
csrss.exe                      416 Services                   0      4,208 K
wininit.exe                    488 Services                   0      5,080 K
``` 
### Kill a Specific Process 
Here I run an instance of notepad with "notepad. I use tasklist with the verbose argument, /v, and I pipe that into the find command with a request of "notepad". I obtain the process identifier number (PID) of notepad. Finally, I use taskkill on the PID number to end the notepad instance. You can see from the last call of tasklist that the notepad instance with PID of 252 is now gone.
```console

C:\Users\IEUser\TEST>notepad

C:\Users\IEUser\TEST>tasklist /v | find "notepad"
notepad.exe                   9176 Console                    2     17,132 K Running
                             0:00:00 win10_commandprompt.txt - Notepad
notepad.exe                    252 Console                    2     14,644 K Running
                             0:00:00 Untitled - Notepad

C:\Users\IEUser\TEST>taskkill /PID 252
SUCCESS: Sent termination signal to the process with PID 252.

C:\Users\IEUser\TEST>tasklist /v | find "notepad"
notepad.exe                   9176 Console                    2     17,132 K Running
                             0:00:00 win10_commandprompt.txt - Notepad

C:\Users\IEUser\TEST>
```
# Network Commands
There are Windows commands for network troubleshooting.
### Testing for Connectivity
You can use ping to test for network connectivity.
```console
C:\Users\IEUser\TEST>ping 127.0.0.1

Pinging 127.0.0.1 with 32 bytes of data:
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128

Ping statistics for 127.0.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
### Getting IP Configuration
The ipconfig command lists the Windows IP Configuration information. I am still learning about network security, so I have redacted all of my ipconfig information. I'm sure I could share more of it, but I want to be careful until I learn more. 
```console
C:\Users\IEUser\TEST>ipconfig
Windows IP Configuration

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : REDACTED
   IPv4 Address. . . . . . . . . . . : REDACTED
   Subnet Mask . . . . . . . . . . . : REDACTED
   Default Gateway . . . . . . . . . : REDACTED

```
### Following Packet Path
The tracert command can be used to follow the path of packets across the internet. Here I use it to get to 8.8.8.8, which is one of Google's DNS servers. I removed the entire path from the results because I am still learning about network security, so I don't want to mistakenly share the wrong thing. 
```console
C:\Users\IEUser\TEST>tracert 8.8.8.8

Tracing route to google-public-dns-a.google.com [8.8.8.8]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  REDACTED
  2     1 ms    <1 ms    <1 ms  REDACTED
  3     1 ms     1 ms     1 ms  REDACTED
  4     2 ms     3 ms     2 ms  REDACTED
  5     5 ms     6 ms     3 ms  REDACTED
  6     7 ms     4 ms     4 ms  REDACTED
  7     4 ms     3 ms     3 ms  REDACTED
  8    20 ms    12 ms    12 ms  REDACTED
  9    16 ms    14 ms    12 ms  REDACTED
 10    16 ms    19 ms    11 ms  REDACTED
 11    11 ms    18 ms    13 ms  google-public-dns-a.google.com [8.8.8.8]
```

### Getting Website Address Info
The nslookup command can be used to get IP address information for a specified website. In this case I used www.professormesser.com, which is an A+ Certificaiton Study Guide Provider.
```console
C:\Users\IEUser\TEST>nslookup www.professormesser.com
Server:  UnKnown
Address:  10.0.2.3

Non-authoritative answer:
Name:    www.professormesser.com
Addresses:  104.20.215.41
          104.20.214.41
```


# Shutting Down the Computer
I hope you enjoyed this survey of Windows commands! Now, I'm finally going to shutdown the computer with the shutdown command and an argument of /f (force).
```console
C:\Users\IEUser\Test>shutdown /f
```




