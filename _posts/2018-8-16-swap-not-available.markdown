---
layout: post
title:  "Linux Swap File Not Available"
date:   2018-08-16 11:57:40 -0700
categories: Troubleshooting
---
You can read about how I broke my [Linux Master Boot Record here]({{ site.baseurl }}{% link _posts/2018-8-16-no-such-partition.markdown %}). I thought I had fixed everything, but then I checked my System Monitor ... See that red box I added? My Swap File isn't available anymore. I have 16 GB of RAM, so it's not surprising that I wasn't experiencing any reductions in performance. In all likelihood, bringing my Swap File back won't do me any good. Still, it's another broken thing, so I'd like to try fixing it.

![swap not available]({{"/assets/swap_not_available/swap_not_available.jpg"}})

First, let's talk a bit more about the Swap File. In Linux RAM is split into 'pages' that can be Swapped between RAM and the Hard Drive. The total amount of available Virtual Memory is equal to the sum of RAM and Swap memory. So Swap is used to get more working memory if RAM is overfilled, but it's also used for system hibernation; to suspend your system, the contents of your RAM are written to the Swap File.

Older computer systems would typically need 2-3 times the size of RAM, but this was back when systems only had 256 MB of RAM. These days you should only have Swap if you're expecting a program to overrun your RAM *or* if you plan to hibernate. I don't picture overruning my RAM, but I would like to be able to hibernate. The [Ubuntu Swap Page] recommends 20 GB of Swap for 16 GB, so that's what I picked back when I was [fixing my Master Boot Record]({{ site.baseurl }}{% link _posts/2018-8-16-no-such-partition.markdown %}).

My Swap exists, but my system doesn't seem to know where it is. This [AskUbuntu Swap question] helped me figure out what to do. In short, I needed to get the UUID for my Swap file and add that to my fstab file. First, I needed to get the UUID of Swap with the blkid command:

```console
sudo blkid
```

This was the output from 'sudo blkid':

```console
[sudo] password for david:
/dev/loop0: TYPE="squashfs"
/dev/loop1: TYPE="squashfs"
/dev/loop2: TYPE="squashfs"
/dev/loop3: TYPE="squashfs"
/dev/loop4: TYPE="squashfs"
/dev/loop5: TYPE="squashfs"
/dev/loop6: TYPE="squashfs"
/dev/loop7: TYPE="squashfs"
/dev/sda1: LABEL="Linux" UUID="363ce6d2-4ee3-4b3c-94cd-9d6c0cc0d816" TYPE="ext4" PARTUUID="151779ce-01"
/dev/sdb1: LABEL="System Reserved" UUID="1AA2AF3D7D7CCDED" TYPE="ntfs" PARTUUID="4178dce4-01"
/dev/sdb2: LABEL="Windows" UUID="F66C6D047CCEE33A" TYPE="ntfs" PARTUUID="4178dce4-02"
/dev/sdb5: UUID="94e06f32-ec74-4c76-928e-a79d539f9214" TYPE="ext4" PARTUUID="4178dce4-05"
/dev/sdb6: UUID="e25632c6-59ad-431c-bb9a-0f14a6bc4cfd" TYPE="swap" PARTUUID="4178dce4-06"
/dev/sdc1: LABEL="Terabyte" UUID="A4DAC86CDAC83C74" TYPE="ntfs" PARTUUID="06fdd344-01"
/dev/loop8: TYPE="squashfs"
/dev/loop9: TYPE="squashfs"
/dev/loop10: TYPE="squashfs"
/dev/loop11: TYPE="squashfs"
/dev/loop12: TYPE="squashfs"
/dev/loop13: TYPE="squashfs"
```

Note that output line 14 has a TYPE of "swap" and a UUID of "e25632c6-59ad-431c-bb9a-0f14a6bc4cfd". I then used cd to get into the /etc directory and vi to open 'fstab'. Note that I used sudo. This is because the file would otherwise be read-only.

```console
cd /etc
sudo vi 'fstab'
```

I wish I'd have saved a before and after picture to help with the explanation, but it didn't cross my mind. All I did was replace the old UUID with the new UUID (from the blkid command). FYI: If your vi editor enters text characters when you try to move around with the arrow keys, you need to add a vimrc file; there's more information on fixing that error on this [StackExchange vimrc page]. Hahaha, I was pretty surprised when my attempts to move the cursor around were adding letters! Anyways, here's the updated fstab file:

![editing fstab]({{"/assets/swap_not_available/adding_UUID_line.jpg"}})

After that edit, the system monitor still showed no swap, so I restarted. And now I have a Swap File again!

![Swap File Restored]({{"/assets/swap_not_available/swap_file_available.jpg"}})

I'm pretty sure there's no actually reason for me to have a Swap File. For example, this is how to check how much Swap you're using.
checking how much swap space you have:

```console
swapon -s
```

This is the swapon output. I'm not actually using any of it.

```console
Filename				Type		Size	Used	Priority
/dev/sdb6                              	partition	20478972	0	-2
```

 I'm pretty convinced that I don't actually need Swap after all, but it was neat learning more about it.

[Ubuntu Swap Page]:https://help.ubuntu.com/community/SwapFaq
[AskUbuntu Swap question]:https://askubuntu.com/questions/194775/swap-not-available-i-must-manually-swapon-after-every-reboot
[StackExchange vimrc page]:https://askubuntu.com/questions/353911/hitting-arrow-keys-adds-characters-in-vi-editor
