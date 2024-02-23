---
layout: post
title:  "Getting Image Metadata on Linux"
date:   2019-1-22 22:42:40 -0700
categories: Linux
---

Metdata is data about data. For example, the metadata of an image will contain information about its creation date. I'm going to do exactly that with ImageMagick, a Linux program that can edit  and analzye images. I already have ImageMagick installed on my computer, but you could install it on a Linux system with this terminal command:

```console
sudo apt-get install imagemagick
```

Here is how I checked for the installed version of ImageMagick:

```console
[23:33] david@Ryzen VirtualBox VMs/ >  identify -version
Version: ImageMagick 6.8.9-9 Q16 x86_64 2018-09-28 http://www.imagemagick.org
Copyright: Copyright (C) 1999-2014 ImageMagick Studio LLC
Features: DPC Modules OpenMP
Delegates: bzlib cairo djvu fftw fontconfig freetype jbig jng jpeg lcms lqr ltdl
lzma openexr pangocairo png rsvg tiff wmf x xml zlib
```

Here's a fun picure of me and a Banana Slug from a hike that I recently went on:

![Dave_and_the_Banana_Slug](/assets/Metadata/Dave_and_the_Banana_Slug.png

Here's the command that I used to obtain all of the metadata:

```console
[23:38] david@Ryzen My_Photos/ >  ls
Dave_and_the_Banana_Slug.jpg
[23:38] david@Ryzen My_Photos/ >  identify -verbose Dave_and_the_Banana_Slug.jpg
Image: Dave_and_the_Banana_Slug.jpg
  Format: JPEG (Joint Photographic Experts Group JFIF format)
  Mime type: image/jpeg
  Class: DirectClass
  Geometry: 2592x1944+0+0
  Resolution: 72x72
  Print size: 36x27
  Units: PixelsPerInch
  Type: TrueColor
  Endianess: Undefined
  Colorspace: sRGB
  Depth: 8-bit
  Channel depth:
    red: 8-bit
    green: 8-bit
    blue: 8-bit
  Channel statistics:
    Pixels: 5038848
    Red:
      min: 0 (0)
      max: 255 (1)
      mean: 105.184 (0.412486)
      standard deviation: 68.4997 (0.268626)
      kurtosis: -0.240451
      skewness: 0.687797
    Green:
      min: 0 (0)
      max: 255 (1)
      mean: 96.6948 (0.379195)
      standard deviation: 63.9978 (0.250972)
      kurtosis: 0.27657
      skewness: 0.795847
    Blue:
      min: 0 (0)
      max: 255 (1)
      mean: 84.8528 (0.332756)
      standard deviation: 67.2398 (0.263686)
      kurtosis: 0.325351
      skewness: 0.937166
  Image statistics:
    Overall:
      min: 0 (0)
      max: 255 (1)
      mean: 95.5772 (0.374812)
      standard deviation: 66.6061 (0.261201)
      kurtosis: 0.161552
      skewness: 0.809653
  Rendering intent: Perceptual
  Gamma: 0.454545
  Chromaticity:
    red primary: (0.64,0.33)
    green primary: (0.3,0.6)
    blue primary: (0.15,0.06)
    white point: (0.3127,0.329)
  Background color: white
  Border color: srgb(223,223,223)
  Matte color: grey74
  Transparent color: black
  Interlace: None
  Intensity: Undefined
  Compose: Over
  Page geometry: 2592x1944+0+0
  Dispose: Undefined
  Iterations: 0
  Compression: JPEG
  Quality: 98
  Orientation: LeftBottom
  Properties:
    date:create: 2019-01-22T23:30:32-08:00
    date:modify: 2018-12-17T18:05:40-08:00
    exif:ColorSpace: 1
    exif:DateTime: 2018:12:15 11:55:27
    exif:DateTimeDigitized: 2018:12:15 11:55:27
    exif:DateTimeOriginal: 2018:12:15 11:55:27
    exif:ExifImageLength: 1944
    exif:ExifImageWidth: 2592
    exif:ExifOffset: 238
    exif:ExifVersion: 48, 50, 50, 48
    exif:ExposureMode: 0
    exif:ExposureProgram: 2
    exif:ExposureTime: 1/229
    exif:Flash: 0
    exif:FlashPixVersion: 48, 49, 48, 48
    exif:FNumber: 170/100
    exif:FocalLength: 210/100
    exif:FocalLengthIn35mmFilm: 21
    exif:GPSInfo: 654
    exif:GPSLatitude: 37/1, 11/1, 9/1
    exif:GPSLatitudeRef: N
    exif:GPSLongitude: 122/1, 0/1, 23/1
    exif:GPSLongitudeRef: W
    exif:ImageLength: 1944
    exif:ImageWidth: 2592
    exif:ISOSpeedRatings: 50
    exif:Make: samsung
    exif:MakerNote: 7, 0, 1, 0, 7, 0, 4, 0, 0, 0, 48, 49, 48, 48, 2, 0, 4, 0, 1,
    0, 0, 0, 0, 32, 1, 0, 12, 0, 4, 0, 1, 0, 0, 0, 0, 0, 0, 0, 16, 0, 5, 0, 1, 0,
    0, 0, 90, 0, 0, 0, 64, 0, 4, 0, 1, 0, 0, 0, 0, 0, 0, 0, 80, 0, 4, 0, 1, 0, 0,
    0, 1, 0, 0, 0, 0, 1, 3, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0
    exif:MaxApertureValue: 153/100
    exif:MeteringMode: 2
    exif:Model: SM-G930V
    exif:Orientation: 8
    exif:ResolutionUnit: 2
    exif:SceneCaptureType: 0
    exif:Software: G930VVRU4BQK4
    exif:WhiteBalance: 0
    exif:XResolution: 72/1
    exif:YCbCrPositioning: 1
    exif:YResolution: 72/1
    jpeg:colorspace: 2
    jpeg:sampling-factor: 2x2,1x1,1x1
    signature: 3196a71ba71059229bde0ec5dbc094c2b049a816b501b4e4002fcdfb9740478d
  Profiles:
    Profile-app4: 5076 bytes
    Profile-exif: 762 bytes
  Artifacts:
    filename: Dave_and_the_Banana_Slug.jpg
    verbose: true
  Tainted: False
  Filesize: 2.348MB
  Number pixels: 5.039M
  Pixels per second: 83.98MB
  User time: 0.060u
  Elapsed time: 0:01.060
  Version: ImageMagick 6.8.9-9 Q16 x86_64 2018-09-28 http://www.imagemagick.org
identify: Invalid SOS parameters for sequential JPEG
`Dave_and_the_Banana_Slug.jpg' @ warning/jpeg.c/JPEGWarningHandler/352.
```
