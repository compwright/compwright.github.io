---
id: 334
title: Cannot run a binary executable from PHP and MAMP?
date: 2012-06-22T13:33:48+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=334
permalink: /2012-06-22/cannot-run-a-binary-executable-from-php-and-mamp/
categories:
  - Notebook
---
I <a href="http://jonathonhill.net/2012-05-18/unshorten-urls-with-php-and-curl/" target="_blank">recently</a> encountered this obscure error when trying to run an external program using PHP (which was bundled with <a href="http://www.mamp.info/en/mamp-pro/" target="_blank">MAMP Pro</a>) on Mac OS X:

> <pre>dyld: Symbol not found: __cg_jpeg_resync_to_restart
Referenced from: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ImageIO.framework/Versions/A/Resources/libTIFF.dylib
Expected in: /Applications/MAMP/Library/lib/libJPEG.dylib
in /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ImageIO.framework/Versions/A/Resources/libTIFF.dylib</pre>

Oddly, other commands execute fine from PHP using `shell_exec()`. Even more strangely, the external program ran fine when executed from a PHP command-line script, just not from MAMP&#8217;s bundled version of Apache!

Thanks to this tidbit from the <a href="http://www.princexml.com/bb/viewtopic.php?f=4&t=3204" target="_blank">PrinceXML forums</a>, the solution was easy:

> MAMP changes the environment variable `$DYLD_LIBRARY_PATH`. Check the file:
> 
> <pre>/Applications/MAMP/Library/bin/envvars</pre>
> 
> and try out if commenting out the two uncommented lines will work for you.