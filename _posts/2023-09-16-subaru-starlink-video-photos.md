---
layout: post
published: true
title: Playing Video in your Subaru Starlink System
date: '2023-09-16 17:53:00 -0400'
comments: true
categories:
  - Miscellaneous
tags:
  - subaru
  - starlink
  - video
  - photos
---

If you own a Subaru Outback or Legacy released in 2015 - 2018 that includes
the Starlink system (the one without Apple Carplay), then you might not know
that you can do the following cool things with a USB drive or SD card.

Note: Don't add files to the navigation/map system SD card, as this will break
the navigation.

* View a slideshow of images
* Upload an image to display when you turn the "screen off"
* Sit back and watch videos while the car is parked (with parking break on)

You can find details on how this is done in the
[2017 Legacy and Outback Subaru Starlink Owners Manual][1], but I'm here to
extract that and tell you what to do.

[1]: https://cdn.subarunet.com/stis/doc/ownerManual/MSA5M1711A_STIS_Index2.pdf

<!--more-->

## General

* The USB thumb drive or SD card should be formatted as FAT 16 or FAT 32
* Maximum number of folders: 3000
* Maximum number of files: 9999
* Maximum files per a folder: 255
* MP3/WMA/AAC files in folders up to 8 levels deep can be played. However, the
  start of playback may be delayed when using discs containing numerous levels
  of folders. For this reason, we recommend creating discs with no more than 2
  levels of folders.

## Music

The player relies on [ID3][2] tags included in the files to identify the title
of the artists, album, track, cover art, etc. If you import CDs or MP3 files you
buy on Bandcamp into the Apple Music app, you edit this metadata for your music,
and even apply cover artwork.

The manual states that the Starlink system supports streaming audio from iPod
devices (classic, iPod Touch 1st - 5th gen, iPod nano 1st - 7th gen, iPhone
1 - 5). I'm using an iPhone 14 Pro and it works fine.

Compatible Files Types:

* MP3 - '.mp3' extension
* WMA - '.wma' extension
* AAC - '.m4a' extension

[2]: https://en.wikipedia.org/wiki/ID3

## Video

Ensure to engage the parking brake when watching video content, otherwise you
will only get a blue video thumbnail.

iPod video is not supported.

The following video codecs are supported:

* WMV9 - WMV or AVI file type
* MPEG4 - MPEG4 or AVI file type
* H.264/AVC - MPEG4 or AVI file type

The following video resolutions are supported:

* [Low definition television][3]
  * 128×96 (MMS-Small - Lowest size recommended for use with 3GPP video)
  * 160×120 (QQVGA - Lowest commonly used video resolution)
  * 176×144 (QCIF Webcam)
  * 320×240 (QVGA, NTSC square pixel)
  * 352×240 (SIF (525) - NTSC-standard VCD / super-long-play DVD)
  * 352×288 (CIF / SIF (625) - PAL-standard VCD / super-long-play DVD)
* [480p][4]
  * 640×480 (480p - 4:3 ratio)
  * 720×480 (480p - 3:2 ratio)
* [576p][5]
  * 720×576 (considered standard definition for PAL regions)

If you are using a Mac, I recommend using [Handbrake][6] to convert your videos.
I used the "Fast 480p30" and "Fast 576p25" presets in Handbrake to convert
videos and they worked just fine.

[3]: https://en.wikipedia.org/wiki/Low-definition_television#Resolutions
[4]: https://en.wikipedia.org/wiki/480p#Resolutions
[5]: https://en.wikipedia.org/wiki/576p
[6]: https://handbrake.fr/

## Images

* The compatible photo file extensions are JPG and JPEG.

## Slideshow

You can view a slideshow of images if you are parked with the emergency parking
break engaged. From the "INFO" menu the USB drive or SD card will show up as
an option, select that option. A menu will display (e.g. "USB Photo") with
options shown to display a Slide Show, with Play Time (Fast, Normal Slow) and
Play Mode (Normal, Random) options.

* Image files can be viewed at the same time that audio files are being played
  back.
* Ensure to engage the parking brake when watching video content. If not, only a
  blue screen will be displayed. Audio, however, can be heard normally.

### Screen Off

You can set an image as the screen off images by uploading images from a USB
drive or SD card.

* When saving the images to a USB or SD card, make sure to place them in an
  'Image' folder located in the root folder of the drive. If this folder name
  is not used, the system cannot download the images. The folder name
  is case sensitive.
* Go to Home > Settings > General, then tap "Customize Screen Off Image"
* After you'll uploaded the images to your system, you can go to Home >
  Settings > Screen Off to make your console display the "Screen Off" image.
