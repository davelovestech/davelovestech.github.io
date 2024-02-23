---
layout: post
title:  "Installing ImageJ on Linux"
date:   2018-10-19 22:42:40 -0700
categories: Linux
---

[ImageJ](https://imagej.nih.gov/ij/) is a scientific image analysis program that was created by the National Institutes of Health. I would like to install ImageJ on my computer so that I can analyze scientific data. I downloaded the 64-bit version from the [ImageJ Download Page](http://rsb.info.nih.gov/ij/download.html).

Here I used the cd command to go to my Downloads directory and then I used the find command to return a list of .zip files.

```console
[14:49] david@Ryzen ~/ >  cd /home/david/Downloads
[14:49] david@Ryzen Downloads/ >  find ./ -type f -name "*.zip"
./ij152-linux64-java8.zip
./old/Sci-Hub.zip
./old/Job Options.zip
./old/fastq_bundle.zip
./old/vagrant_2.1.2_linux_amd64.zip
./old/Berkeley_Machine_Learning_Python-master.zip
./old/genome_David_Halvorsen_v4_Full_20180725194004.zip
./old/fastq_bundle (1).zip
./old/fastq_bundle (2).zip
```

The ImageJ file is called "ij152-linux64-java8.zip." It is a zip file, so I'll need to unzip it with this command:

```console
[14:50] david@Ryzen Downloads/ >  unzip ij152-linux64-java8.zip
Archive:  ij152-linux64-java8.zip
   creating: ImageJ/
  inflating: ImageJ/ij.jar           
  inflating: ImageJ/ImageJ           
   creating: ImageJ/images/
 extracting: ImageJ/images/icon.png  
```

Please note that I only included a few of the lines of output (there were many, many lines). Now I can navigate to the ImageJ folder and run the ImageJ program from the terminal with this command:

```console
[14:52] david@Ryzen Downloads/ >  cd ImageJ
[14:53] david@Ryzen ImageJ/ >  ls
ImageJ  ij.jar  images  jre  luts  macros  plugins
[14:53] david@Ryzen ImageJ/ >  ./ImageJ
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option PermSize=128m; support was removed in 8.0
Java HotSpot(TM) 64-Bit Server VM warning: Using incremental CMS is deprecated and will likely be removed in a future release
```
