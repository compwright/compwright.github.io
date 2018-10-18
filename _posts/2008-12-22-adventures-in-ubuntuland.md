---
id: 12
title: Adventures in Ubuntuland
date: 2008-12-22T11:37:17+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=12
permalink: /2008-12-22/adventures-in-ubuntuland/
categories:
  - Notebook
tags:
  - Eclipse
  - Linux
  - Opera
  - Ubuntu
---
My two-year-old HP laptop is getting slower and slower. Time for an XP re-install, but that&#8217;s a big ordeal. I opted to give Linux another try (maybe the third time will be the charm).

I installed and configured Eclipse, but it was running super-slow. A quick Google search [revealed a simple trick](http://blog.thadeusb.com/2008/10/20/eclipse-is-slow-laggy-on-linux/):

> The eclipse launcher resides at
> 
> /bin/eclipse
> 
> yet the actual executable for eclipse is at
> 
> /usr/lib/eclipse/eclipse
> 
> If you just bypass the wrapper script in /bin/eclipse and execute the binary, you will have increased speed for eclipse. 

That seemed to take care of that.

I&#8217;m a dedicated Opera-holic. I simply can&#8217;t/won&#8217;t do without Opera&#8217;s super-useful e-mail client. Of course it runs fine on Linux, but often the fonts look terrible on many websites, even with the Microsoft core TrueType fonts installed.

Soo, after another [quick Google search](http://list.opera.com/pipermail/opera-linux/2004-August/008040.html) here&#8217;s what I found:

> If you are sure that you have enough truetype fonts available (through
  
> xft. &#8216;opera -debugfont&#8217; will list the ones opera finds on startup), you can try disabling X font support by adding this line to the [User Prefs] section of one of the opera configuration files (e.g. /etc/opera6rc):
> 
> `Enable Core X Fonts=0` 

Voila, nice fonts in Opera!