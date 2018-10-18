---
id: 126
title: 10 Tips for Optimizing MySQL Queries
date: 2009-05-04T18:43:35+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=126
permalink: '/2009-05-04/10-tips-for-optimizing-mysql-queries-that-don%e2%80%99t-suck/'
categories:
  - Notebook
tags:
  - 20bits
  - EXPLAIN
  - MySQL
  - Optimizing
  - Queries
---
In my quest to understand MySQL&#8217;s EXPLAIN statement and to learn more strategies for optimizing queries, I came across this excellent blog post from 20bits by Jesse Farmer:

<http://20bits.com/articles/10-tips-for-optimizing-mysql-queries-that-dont-suck/>

In summary:

  1. <a href="http://vegan.net/tony/supersmack/" target="_blank">Benchmark</a>, <a href="http://httpd.apache.org/docs/2.2/programs/ab.html" target="_blank">benchmark</a>, [benchmark](http://sysbench.sourceforge.net/)!
  2. <a href="http://mtop.sourceforge.net/" target="_blank">Profile</a>, profile, profile!
  3. Tighten Up Your Schema
  4. Partition Your Tables
  5. Don’t Overuse Artificial Primary Keys
  6. Learn Your Indices
  7. SQL is Not C
  8. Understand your engines
  9. MySQL specific shortcuts
 10. Read Peter Zaitsev’s <a href="http://mysqlperformanceblog.com/" target="_blank">MySQL Performance Blog</a>