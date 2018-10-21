---
title: Just-in-time $GLOBALS
date: 2012-07-10 17:22:00 -04:00
permalink: "/2012-07-10/jit-globals/"
categories:
- Notebook
tags:
- globals
- PHP
id: 354
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=354
---

**Update**—Turns out, this isn&#8217;t actually a bug. Rasmus Lerdorf has already explained this behavior in response to <a href="https://bugs.php.net/bug.php?id=54131#1299042912" target="_blank">a bug report</a>:

> This is not a bug. It is a documented optimization feature. See <http://php.net/manual/en/ini.core.php> and look for the section on <a href="http://www.php.net/manual/en/ini.core.php#ini.auto-globals-jit" target="_blank"><code>auto_globals_jit</code></a> with the big pink warning which says, &#8220;Usage of SERVER and ENV variables is checked during compile time so using them through e.g. variable variables will not cause their initialization.&#8221;

* * *

I encountered an odd bug in PHP 5.3.10 while attempting to loop through all the major superglobals (`$_SERVER`, `$_GET`, `$_POST`, etc). Here is what I found, if you can shed any light on why this happens, or if you can confirm that it isn&#8217;t just me, I would greatly appreciate it!

## Test Case

<pre>&lt;?php
var_dump($GLOBALS['_SERVER']);
// These behave the same way:
// $var = '_SERVER'; var_dump($$var);
// var_dump(${$var});</pre>

## Expected Behavior

<pre>$GLOBALS["_SERVER"]: array(27) {
...
}</pre>

## Actual Behavior

<pre>NULL</pre>

## Workaround

What makes this so strange is that if you access `$_SERVER` (or `${'_SERVER'}`) directly in the code anywhere—even if it doesn&#8217;t actually get executed—this bug does not manifest itself! In other words, this works just fine:

<pre>&lt;?php
var_dump($GLOBALS['_SERVER']);
exit;
$var = $_SERVER;</pre>