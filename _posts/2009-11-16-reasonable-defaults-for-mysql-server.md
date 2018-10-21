---
title: Reasonable Defaults for MySQL Server
date: 2009-11-16 01:05:02 -05:00
permalink: "/2009-11-16/reasonable-defaults-for-mysql-server/"
categories:
- Notebook
tags:
- Hardening
- MySQL
id: 195
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=195
---

[Jeremy Zawodny](http://jeremy.zawodny.com/blog/) posted [yet another great article](http://www.linux-mag.com/id/7615) over at Linux Magazine on some improved defaults that can save you a lot of grief when your network fails intermittently. In summary:

**Faster replication heartbeat** (old default is 3600 seconds = 1 hour):

<pre style="padding-left: 30px">slave_net_timeout 10</pre>

**Disable DNS hostname lookups:**

<pre style="padding-left: 30px">skip-name-resolve</pre>

**Sane connection timeout** (may need to be raised if your network is flaky):

<pre style="padding-left: 30px">connect_timeout 5</pre>

**Disable host blacklisting** after x number of failed connections:

<pre style="padding-left: 30px">max_connect_errors 1844674407370954751</pre>