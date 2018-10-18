---
id: 129
title: Skype hijacks port 80
date: 2009-05-14T10:20:20+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=129
permalink: /2009-05-14/skype-hijacks-port-80/
categories:
  - Notebook
tags:
  - Apache
  - Port 80
  - Skype
---
Imagine my dismay and perplexity today when Apache suddenly stopped working on my development PC. Checking the Windows event log revealed this error:

<pre>The Apache service named  reported the following error:
&gt;&gt;&gt; (OS 10048)Only one usage of each socket address (protocol/network address/port) is normally permitted.  : make_sock: could not bind to address 0.0.0.0:80</pre>

Aha! Another service is using port 80, preventing Apache from binding to that port. But what? I wondered if I had caught some terrible malware serving out who-knows-what&#8230;

However, a quick Google search revealed Skype as the culprit!

[<img class="alignnone size-medium wp-image-130" title="skype" src="/wp-content/uploads/2009/05/skype-300x242.png" alt="skype" width="300" height="242" srcset="/wp-content/uploads/2009/05/skype-300x242.png 300w, /wp-content/uploads/2009/05/skype.png 706w" sizes="(max-width: 300px) 100vw, 300px" />](/wp-content/uploads/2009/05/skype.png)