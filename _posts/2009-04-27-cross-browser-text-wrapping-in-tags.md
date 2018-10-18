---
id: 117
title: 'Cross-browser text wrapping in <pre> tags'
date: 2009-04-27T07:50:43+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=117
permalink: /2009-04-27/cross-browser-text-wrapping-in-tags/
categories:
  - Notebook
tags:
  - '&lt;pre&gt;'
  - CSS
  - CSS3
  - IE7
  - Text wrapping
---
Until CSS3 is widely supported, if you want to wrap text inside a <pre> tag you can do it this way:

<pre>pre {
  white-space: pre-wrap; /* css-3 */
  white-space: -moz-pre-wrap !important; /* Mozilla, since 1999 */
  white-space: -pre-wrap; /* Opera 4-6 */
  white-space: -o-pre-wrap; /* Opera 7 */
  word-wrap: break-word; /* Internet Explorer 5.5+ */
  white-space: normal;
  width:99%;
}</pre>

Note that `white-space:normal` at the end makes IE behave, and `width:99%` prevents the dreaded horizontal scrollbar.

Thanks to &#8220;Deng Zhi&#8221; who commented on [Wrapping Text Inside Pre Tags](http://www.longren.org/2006/09/27/wrapping-text-inside-pre-tags/).