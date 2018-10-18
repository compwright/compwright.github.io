---
id: 106
title: jQuery hoverIntent plugin
date: 2009-04-01T04:54:51+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=106
permalink: /2009-04-01/jquery-hoverintent-plugin/
categories:
  - Notebook
tags:
  - Events
  - Hover
  - Javascript
  - jQuery
  - Plugin
---
<a href="http://cherne.net/brian/resources/jquery.hoverIntent.html" target="_blank">hoverIntent</a> is a plug-in that works like (and was derived from) jQuery&#8217;s built-in hover. However, instead of immediately calling the onMouseOver function, it waits until the user&#8217;s mouse slows down enough before making the call. Why? To delay or prevent the accidental firing of animations or ajax calls. Simple timeouts work for small areas, but if your target area is large it may execute regardless of intent.