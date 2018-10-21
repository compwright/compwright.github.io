---
title: Auto-start a shell script on Ubuntu Server
date: 2009-04-23 08:13:21 -04:00
permalink: "/2009-04-23/auto-start-a-shell-script-on-ubuntu-server/"
categories:
- Notebook
tags:
- Autostart
- init.d
- Linux
- Shell
- Ubuntu
id: 115
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=115
---

Got a shell script that you want automatically run at bootup on Ubuntu Server Edition? Here&#8217;s how:

  1. Create a script in the /etc/init.d/ directory
  2. Make the script executable 
    <pre>$ sudo chmod +x /etc/init.d/myscript.sh</pre>

  3. Make the script start at bootup 
    <pre>$ sudo update-rc.d myscript.sh defaults</pre>

Note: the option “defaults” puts a link to start your script in runlevels 2, 3, 4 and 5, and puts a link to stop in runlevels 0, 1 and 6.

Referenced from:

  * <a href="http://ubuntu.wordpress.com/2005/09/07/adding-a-startup-script-to-be-run-at-bootup/" target="_blank">Adding a startup script to be run at bootup</a>
  * <a href="http://stringofthoughts.wordpress.com/2009/04/16/adding-removing-shell-scripts-ubuntu-810/" target="_blank">Adding / Removing Shell scripts (Ubuntu 8.10)</a>