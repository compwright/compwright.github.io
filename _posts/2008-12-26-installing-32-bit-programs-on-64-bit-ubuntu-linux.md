---
title: Installing 32-bit programs on 64-bit Ubuntu linux
date: 2008-12-26 06:08:22 -05:00
permalink: "/2008-12-26/installing-32-bit-programs-on-64-bit-ubuntu-linux/"
categories:
- Notebook
tags:
- 32-bit
- 64-bit
- Linux
- Ubuntu
id: 15
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=15
---

So far so good on my experimental switch to Ubuntu Linux from Windows XP.

I&#8217;m running 64-bit linux because my laptop&#8217;s Intel CPU supports it. However, many but not all programs available for linux are available in 64-bit versions. Last few days I&#8217;ve needed to install a few programs that don&#8217;t have 64-bit versions available, namely, Adobe Reader 8 and Amazon&#8217;s MP3 downloader. Here&#8217;s what I have learned:

  * Linux&#8217;s package managers are **made** to balk at installing 32-bit packages on 64-bit OSs. That is simply to remind you to check to see if a 64-bit version is available, (often it is).
  * 64-bit hardware and OSs are backward-compatible with 32-bit software.

So, most of the time all you have to do is tell the package manager &#8220;you know what you&#8217;re doing&#8221; and install anyway:

`$ sudo dpkg -i --force-all AdobeReader_enu-8.1.3-1.i386.deb`

[<img class="alignnone size-large wp-image-19" title="screenshot-adobereader1" src="/wp-content/uploads/2008/12/screenshot-adobereader1-1024x640.png" alt="screenshot-adobereader1" width="100%" srcset="/wp-content/uploads/2008/12/screenshot-adobereader1-1024x640.png 1024w, /wp-content/uploads/2008/12/screenshot-adobereader1-300x187.png 300w, /wp-content/uploads/2008/12/screenshot-adobereader1.png 1440w" sizes="(max-width: 1024px) 100vw, 1024px" />](/wp-content/uploads/2008/12/screenshot-adobereader1.png)

Which is how I got Adobe Reader working. Now for Amazon&#8217;s MP3 downloader. The purpose of the downloader is to make it easy and convenient to download whole albums at once, file it, and allow you to pause/resume downloading if needed.

Honestly, I was surprised and delighted to find that Amazon even makes a Linux version. So, I tried to install it. It complained about a bunch of libboost libraries being missing. So I headed over to synaptic and installed all the libboost_*-1.34.1 packages and was then able to install:

`$ sudo dpkg -i --force-all amazonmp3.deb`

When I tried to run it, it wouldn&#8217;t open. I tried opening it from the terminal and it said it couldn&#8217;t open some of the libboost libraries that I installed. Somewhere I read that you can use these commands to determine the architecture of an installed program or library:

`$ file /usr/bin/amazonmp3<br />
$ file /usr/lib/libboost-thread1.34.1`

Well, amazonmp3 is 32-bit and the libboost libraries I installed were all 64-bit. So, I installed the [getlibs package](http://ubuntuforums.org/showthread.php?t=474790) and ran it:

`$ sudo getlibs /usr/bin/amazonmp3`

It downloaded a **bunch** of 32-bit packages and now amazonmp3 works like a charm.

[<img class="alignnone size-large wp-image-23" title="screenshot-amazonmp3" src="/wp-content/uploads/2008/12/screenshot-amazonmp3-1024x640.png" alt="screenshot-amazonmp3" width="100%" srcset="/wp-content/uploads/2008/12/screenshot-amazonmp3-1024x640.png 1024w, /wp-content/uploads/2008/12/screenshot-amazonmp3-300x187.png 300w, /wp-content/uploads/2008/12/screenshot-amazonmp3.png 1440w" sizes="(max-width: 1024px) 100vw, 1024px" />](/wp-content/uploads/2008/12/screenshot-amazonmp3.png)

Kudos to Amazon for their DRM-free MP3s and for making a cross-platform downloader that makes using their service easy!