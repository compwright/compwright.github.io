---
title: Arbitrary-length base conversion in PHP
date: 2009-09-04 09:31:08 -04:00
permalink: "/2009-09-04/arbitrary-length-base-conversion-in-php/"
categories:
- Notebook
tags:
- Base conversion
- Binary
- Decimal
- Extended arithmetic
- GMP
- PHP
id: 189
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=189
---

After some digging, I found a great way to convert number bases when dealing with arbitrary length integers (esp. integers > 32 bits):

`return gmp_strval(gmp_init($n, 2), 10);`

This will convert a large base 2 (binary) number to base 10 (decimal).