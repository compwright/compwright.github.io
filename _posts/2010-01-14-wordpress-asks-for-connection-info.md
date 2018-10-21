---
title: What to do when WordPress 2.8+ asks for connection info to upgrade a plugin
date: 2010-01-14 03:28:29 -05:00
permalink: "/2010-01-14/wordpress-asks-for-connection-info/"
categories:
- Notebook
tags:
- Wordpress
id: 221
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=221
---

I was getting this when trying to upgrade a plugin automatically within [WordPress](http://wordpress.org):

<img class="alignnone" title="Connection info needed" src="http://www.chrisabernethy.com/wp-content/uploads/2008/12/connection_info_needed.jpg" alt="" width="470" height="262" />

Normally this would be a filesystem permission error. You have to [make sure the `wp-content/plugins` folder is owned by the user apache is running as](http://www.chrisabernethy.com/why-wordpress-asks-connection-info/). However, that didn&#8217;t change a thing.

Further googling revealed that I needed to [add this constant](http://www.kgarner.com/blog/archives/2009/06/13/direct-auto-update-on-wordpress-2-8/) in my `wp-config.php` file:

<pre class="php">define('FS_METHOD', 'direct');</pre>