---
layout: post
title:  "Escaping Linux Emergency Mode"
date:   2019-1-24 13:42:40 -0700
categories: Troubleshooting
---

I recently tried to boot up my Ubuntu 16.04 system, but I didn't reach the normal login window. I was greeted with Emergency Mode:

![Welcome_to_Emergency_Mode](/assets/Emergency_Mode/Welcome_to_Emergency_Mode.jpg)

^I should specify that the image above is from [this Stack Exchange post](https://askubuntu.com/questions/646414/welcome-to-emergency-mode-think-it-is-a-fsck-problem). I regrettably forgot to take a picture of my initial error, but it was the same one that is imaged above.

I had never seen this kind of error before, so I started playing with it. I could reboot my computer with this command:

```console
sudo systemctl reboot
```

Running the following command returned a few thousand lines of log information:

```console
journalctl -xb
```

I briefly read through some of the log file, but it was so long that I doubted I'd be able to find anything useful ... UNLESS I knew what I was looking for. So I Googled for "Linux welcome to emergency mode" and I found [this Stack Exchange post](https://askubuntu.com/questions/646414/welcome-to-emergency-mode-think-it-is-a-fsck-problem). The first suggestion was to run fsck from Ubuntu Live.

I have a flash drive with Ubuntu Live, but I expected this method to take a long time, so I read through the StackExchange page a little more. Luckily I read a bit more and found users discussing a Windows 10 / Ubuntu 16 case of this problem. I was excited to find this part of the discussion because the system having this problem IS a Windows 10 / Ubuntu 16.04 dual boot!

Apparently Windows 10 can lock a partition that its supposed to share with Linux AND THEN Linux can't boot because it's expecting the shared partition to be free. I remembered that I had recently added my Terabyte HDD to my Linux fstab file and took a guess that I was experiencing the situation described in the StackExchange post, so I ran this command to edit my fstab file:

```console
sudo nano /etc/fstab
```

Below is a picture of my fstab file. Please forgive the weird angle:

![fstab](/assets/Emergency_Mode/fstab.jpg)

The last line of the fstab file is the line for my shared Terabyte HDD. I took a guess that I was experiecing the same problem that the StackExchange comments mentioned, so I deleted that line. I saved the fstab file and then exited back to Emergency Mode. I thought it had a good chance of fixing my problem, so that's when I rebooted with this command:

```console
sudo systemctl reboot
```

Hooray! That fixed my problem! After bootup I was greeted with the normal Ubuntu login screen :)
