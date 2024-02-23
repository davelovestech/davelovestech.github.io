---
layout: post
title:  "Rotating Images from the Terminal"
date:   2018-08-16 11:57:40 -0700
categories: Linux
---
Have you ever wanted to rotate an image by a specific number of degrees? Prior to today, I've only ever had the desire while in Windows. Paint only lets you rotate by a # of degrees that is divisible by 90, so I would use PowerPoint to rotate and then save that image from PowerPoint ... yup, it's a silly/weird way of doing things, but it was always quick, so I stuck with it. Here's an oddly angled photo that I'd like to fix:

![wrong angle image]({{"/assets/ubuntu_rotate_images/wrong_angle.jpg"}})

Today I tried rotating an image in Linux and I learned a lot that I'd like to share with you. Some quick Googling led me to an [Ubuntu package named "imgp"]. It is a "superfast batch image resizer and rotator". I downloaded the tar.gz file and realized that I had forgotten the terminal command for opening that kind of file. (I keep forgetting the command for that file, so I've decided to write an article about it.) This website shows you how to [open a tar.gz file in Linux].

Having typed it out below, I'll hopefully remember the command now!

```console
tar -zxvf imgp_2.5.orig.tar.gz
```

![opening tar.gz]({{"/assets/ubuntu_rotate_images/opening_tar_dot_gz.jpg"}})

I had no idea how the program worked, so I tried running it. That yielded the available arguments (this has worked for all the programs I've been confused about so far).

![imgp arguments]({{"/assets/ubuntu_rotate_images/imgp_arguments.jpg"}})

Reading through the arguments, I found the '-- rotate' argument and used that. I tried a variety of degree arguments (-3 <-> 2) and finally found a good angle. It was easy enough!

![rotating commands]({{"/assets/ubuntu_rotate_images/imgp_terminal_rotating.jpg"}})

Here's the better-angled output:

![right angle image]({{"/assets/no-such_partition/BC2_bootloader_fail.jpg"}})

[Ubuntu package named "imgp"]:https://launchpad.net/ubuntu/bionic/+package/imgp
[open a tar.gz file in Linux]:https://www.cyberciti.biz/faq/howto-open-a-tar-gz-file-in-linux-unix/
