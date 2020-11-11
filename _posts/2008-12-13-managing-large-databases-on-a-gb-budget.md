---
title: Managing large MySQL databases on a GB budget
date: 2008-12-13T11:17:46.000-05:00
permalink: "/2008-12-13/hello-world/"
categories:
- Notebook
tags:
- MySQL
id: 1
author: Jonathon
excerpt: |
  I just wanted to quickly mention something about how MySQL handles out-of-disk-space situations when importing an SQL file. Here's my situation:
  I have an 80GB hard disk,
  7GB free space,
  and am loading a 2GB SQL database dump .

  No problem, right? Well not always. I started it importing and went to bed. In the morning it hadn't finished and I was getting out-of-space notifications on my taskbar.
layout: post
guid: http://jonathonhill.net/?p=1

---
What happens when MySQL runs out of disk space while importing a large SQL file?

Take a situation I experienced:

* 80GB hard disk,
* 7GB free space,
* Loading a 2GB SQL database dump.

No problem, right? I started the import and went to bed. In the morning it hadn't finished, and I was getting out-of-space notifications on my taskbar.

Thankfully, I'd seen this before and knew all I had to do was run this simple query:

    RESET MASTER;

While I don't fully understand all the ins-and-outs of how it works, MySQL has a binary log that can grow considerably larger than your database. This simple query empties that log.

I ran that, and voila, 5GB free instantly.

All this led to an interesting discovery: after stopping the import on the command line, MySQL gave this error message:

    ERROR 20 (HY000) at line 2373: Disk is full writing '.\mydatabase\mytable.MYD' (Errcode: 28). Waiting for someone to free space... Retry in 60 secs

Apparently, it had been waiting all night rather than crashing, and would have continued had I freed up disk space before stopping the import.

But I guess it's good that I did, because otherwise I would have never known that MySQL did this!