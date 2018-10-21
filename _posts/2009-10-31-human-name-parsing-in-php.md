---
title: Human Name Parsing in PHP
date: 2009-10-31 14:25:27 -04:00
permalink: "/2009-10-31/human-name-parsing-in-php/"
categories:
- Notebook
tags:
- Human Names
- nameparse
- Parsing
- PHP
id: 193
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=193
---

Parsing human names are not exactly easy, but they can be done. Keith Beckman&#8217;s [`nameparse.php`](http://alphahelical.com/code/misc/nameparse/) is an excellent PHP library for doing this.

[**Download nameparse.php**](http://alphahelical.com/code/misc/nameparse/nameparse.php.txt)

> `nameparse.php` can recognize names in &#8220;\[title]first[middles]last[,\]\[suffix\]&#8221; and &#8220;last,first\[middles\]\[,\][suffix]&#8221; forms, which, when you think about it, cover most if not all well-formed name input formats. `nameparse.php` handles last names of arbitrary complexity, such as &#8220;bin Laden&#8221;, &#8220;van der Vort&#8221;, and &#8220;Garcia y Vega&#8221;, as well as middle names of arbitrary size and complexity, differentiating between most last names and the first or middle names or initials preceding them.
> 
> An example of names correctly parse by nameparse.php:
> 
>   * Doe, John. A. Kenneth III
>   * Velasquez y Garcia, Dr. Juan, Jr.
>   * Dr. Juan Q. Xavier de la Vega, Jr.
> 
> To use, simple `include()` or `require()` `nameparse.php` and call `parse_name($string)` on any name. `parse_name()` returns an associative array of all name segments found of &#8220;title&#8221;,&#8221;first&#8221;,&#8221;middle&#8221;,&#8221;last&#8221;, and &#8220;suffix&#8221;. Do note that no spelling, capitalization, or punctuation of titles, prefixes, or suffixes is normalized. That is, every token remains as entered: `nameparse.php` is a semantic parser only. If you want orthographic or other normalization, you&#8217;ll have to postprocess the output. However, since the name is now semantically parsed, such postprocessing is (for applications which require it) simple.
> 
>  `print_r(parse_name('Velasquez y Garcia, Dr. Juan Q. Xavier III'));` 
> 
> yields . . .
> 
> <pre>Array
(
    [title] =&gt; Dr.
    [first] =&gt; Juan
    [middle] =&gt; Q. Xavier
    [suffix] =&gt; III
    [last] =&gt; Velasquez y Garcia
)</pre>