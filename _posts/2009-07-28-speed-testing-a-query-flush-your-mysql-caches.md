---
title: Speed-testing a query? Flush your MySQL caches
date: 2009-07-28 15:40:23 -04:00
permalink: "/2009-07-28/speed-testing-a-query-flush-your-mysql-caches/"
categories:
- Notebook
tags:
- Benchmarking
- Cache
- Flush
- MySQL
id: 154
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=154
---

Thanks to the <a href="http://www.mysqlperformanceblog.com/2007/09/12/query-profiling-with-mysql-bypassing-caches/" target="_blank">MySQL Performance Blog</a> for this tip:

`RESET QUERY CACHE;<br />
FLUSH TABLES;<br />
SET GLOBAL key_buffer_size=0;<br />
SET GLOBAL key_buffer_size=14691328;`