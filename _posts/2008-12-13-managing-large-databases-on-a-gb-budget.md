---
title: Managing large MySQL databases on a GB budget
date: 2008-12-13 11:17:46 -05:00
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

Hello World of blogging!

I just wanted to quickly mention something about how MySQL handles out-of-disk-space situations when importing an SQL file. Here&#8217;s my situation:

  * I have an 80GB hard disk,
  * 7GB free space,
  * and am loading a 2GB SQL database dump .

No problem, right? Well not always. I started it importing and went to bed. In the morning it hadn&#8217;t finished and I was getting out-of-space notifications on my taskbar.

Thankfully, I&#8217;d seen this before and knew all I had to do was run this simple query:

`RESET MASTER;`

While I don&#8217;t fully understand all the ins-and-outs of how it works, MySQL has a binary log that can grow considerably larger than your database. This simple query empties that log.

I ran that, and voila, 5GB free instantly. But here&#8217;s the interesting thing that I discovered.  After stopping the import on the command line, MySQL gave this error message:

`ERROR 20 (HY000) at line 2373: Dis is full writing '.\mydatabase\mytable.MYD' (Errcode: 28). Waiting for someone to free space... Retry in 60 secs`

So apparently, it had been waiting all night rather than crashing. Unfortunately, I stopped it before freeing disk space and restarted it from the beginning. But I guess that&#8217;s good, because otherwise I would have never known that MySQL did this!