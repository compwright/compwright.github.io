---
id: 234
title: Avoid the void(0) in IE6
date: 2010-04-07T11:10:40+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=234
permalink: /2010-04-07/avoid-the-void0-in-ie6/
categories:
  - Notebook
tags:
  - IE6
  - Javascript
  - void
---
I recently learned the hard way that `<a href="javascript:void(0)">` doesn&#8217;t work as expected in IE6. The solution is to use the familiar `#` but make sure your `onclick` event returns **`false`**:

`<a href="#" onclick="aFunction();return false;">Link</a>`

<a href="http://blog.reindel.com/2006/08/11/a-hrefjavascriptvoid0-avoid-the-void/" target="_blank">d&#8217;bug has a great writeup on <em>why</em> this works and the other doesn&#8217;t</a>.