---
title: How FriendFeed uses MySQL to store schema-less data
date: 2009-03-20 01:30:55 -04:00
permalink: "/2009-03-20/how-friendfeed-uses-mysql-to-store-schema-less-data/"
categories:
- Notebook
tags:
- MySQL
- Scaling
- Schema-less
id: 90
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=90
---

This is beautiful! Check out <a title="How FriendFeed uses MySQL to store schema-less data" href="http://bret.appspot.com/entry/how-friendfeed-uses-mysql" target="_blank">this post</a> on Bret Taylor&#8217;s blog:

> We use MySQL for storing all of the data in FriendFeed. Our database has grown a lot as our user base has grown. We now store over 250 million entries and a bunch of other data, from comments and &#8220;likes&#8221; to friend lists.
> 
> As our database has grown, we have tried to iteratively deal with the scaling issues that come with rapid growth. We did the typical things, like using read slaves and memcache to increase read throughput and sharding our database to improve write throughput. However, as we grew, scaling our existing features to accomodate more traffic turned out to be much less of an issue than adding new features.
> 
> &#8230;
> 
> After some deliberation, we decided to implement a &#8220;schema-less&#8221; storage system on top of MySQL rather than use a completely new storage system. This post attempts to describe the high-level details of the system. We are curious how other large sites have tackled these problems, and we thought some of the design work we have done might be useful to other developers.
> 
> &#8230;