---
id: 405
title: Write tests, you might learn something
date: 2013-04-05T11:43:28+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=405
permalink: /2013-04-05/write-tests-you-might-learn-somethin/
categories:
  - Notebook
tags:
  - aes-256
  - mcrypt
  - phpunit
  - testing
---
Writing unit tests for your code is widely regarded as a best practice. There are many excuses for _not_ writing tests: time, cost, and the fun factor. Excuses aside, there are some <a href="http://twitpic.com/7c4k9m" target="_blank">Good Reasons</a> to write tests for your code. I just discovered one today.

## Write tests, you might learn something&#8230;faster

I&#8217;m working on an API integration that requires a bit of AES-256 encryption. Getting that worked out in PHP took some surprising turns, so just to be sure that I was getting all the details right (such as initialization vector size) I decided to write a unit test for my mcrypt wrapper library.

Since this library had worked well in other projects, imagine my surprise when the first AES-256 encrypt-decrypt test failed:

<pre>$ phpunit *
PHPUnit 3.7.8 by Sebastian Bergmann.

.F..

Time: 0 seconds, Memory: 2.50Mb

There was 1 failure:

1) EncryptionAes256Test::testEncryptDecrypt with data set #0 ('ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefhijklmnopqrstuvwxyz')
Failed asserting that two strings are equal.
--- Expected
+++ Actual
@@ @@
-'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefhijklmnopqrstuvwxyz'
+Binary String: 0x4142434445464748494a4b4c4d4e4f505152535455565758595a61626364656668696a6b6c6d6e6f707172737475767778797a00000000000000000000000000

/Users/jhill/Sites/mrtz_010/Source/tests/EncryptionTest.php:53

FAILURES!
Tests: 4, Assertions: 7, Failures: 1.</pre>

Echoing out the decrypted string proved just as bizarre:

<pre>string(51) "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefhijklmnopqrstuvwxyz"
string(64) "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefhijklmnopqrstuvwxyz"</pre>

It seems that <a href="http://www.php.net/manual/en/function.mcrypt-decrypt.php#54734" target="_blank">mcrypt pads the string with enough null characters (<code>\0</code>) to match the block size</a>, and unlike strings in C, PHP strings do not necessarily end with the first null character.

The fix was obvious:

<pre>$decrypted = rtrim($decrypted, "\0");</pre>

Guess what? By investing time in a simple unit test, I unexpectedly learned something new about PHP, AND saved myself quite a bit of time debugging an API call that would not have worked.

I&#8217;d say it was worth it.