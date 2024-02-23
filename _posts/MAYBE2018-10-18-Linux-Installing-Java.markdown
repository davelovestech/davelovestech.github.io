---
layout: post
title:  "Installing Java on Linux"
date:   2018-10-18 22:42:40 -0700
categories: Linux
---

[ImageJ](https://imagej.nih.gov/ij/) is a scientific image analysis program that was created by the National Institutes of Health. I would like to install ImageJ on my computer so that I can analyze scientific data.

I have chosen the 64-bit Java version of ImageJ to be run on my Linux Ubuntu 16.04 operating system. Before installing ImageJ, I would like to check if Java is installed on my system with this terminal command:

```console
[14:23] david@Ryzen ~/ >  java -version
The program 'java' can be found in the following packages:
 * default-jre
 * gcj-5-jre-headless
 * openjdk-8-jre-headless
 * gcj-4.8-jre-headless
 * gcj-4.9-jre-headless
 * openjdk-9-jre-headless
Try: sudo apt install <selected package>
```

As you can see from the output, Java is not installed on my system. Here are the commands that I used to install Java on my system:

```console
sudo apt-get update
sudo apt-get install default-jre
```

Remember the version command that I used from earlier? Now it returns information about my successfull Java install!

```console
[14:39] david@Ryzen ~/ >  java -version
openjdk version "1.8.0_191"
OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-0ubuntu0.16.04.1-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
```
