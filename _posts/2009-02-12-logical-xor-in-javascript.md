---
id: 78
title: Logical XOR in Javascript
date: 2009-02-12T22:21:06+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=78
permalink: /2009-02-12/logical-xor-in-javascript/
categories:
  - Notebook
tags:
  - Boolean Logic
  - Javascript
---
XOR (exclusive OR) is a boolean operation, like `&&` and `||`, but with the following logic: It is true if the expression on either side is true (like `||`), but not if both sides are true (like `&&`).

Sometimes, in your JavaScript, you might want to do the following:

    if( foo XOR bar ) { ... }

Unfortunately, JavaScript does not have a logical XOR operator. <a href="http://www.howtocreate.co.uk/xor.html" target="_blank">This article</a> covers several methods of performing an XOR manually, and concludes with a surprisingly succinct way of doing it, summarized below.

> ## The cleanest and best
> 
> The XOR operation can be described as &#8220;return true if the two boolean operands do not have the same value&#8221;. So the actual response that is wanted is just this:
> 
>     if( foo != bar ) { ... }
> 
> That works if the two operands are both boolean true/false. However, to allow for other variable types (strings, expressions, etc.), it cannot work, since <var>&#8216;string&#8217;</var> does not equal <var>/regex/</var>, even though the XOR operation would be expected to return false. So the operands need to be converted into boolean values first. This can be done like this:
> 
>     foo ? true : false
> 
> However, it is easier and quicker to simply use the `!` operator. This will give the opposite value &#8211; false instead of true, but as long as both operands get the same treatment, this will not affect the end result. So the simplest, cleanest version of the XOR operator, that works with any data types, including expressions, is this:
> 
>     if( !foo != !bar ) { ... }