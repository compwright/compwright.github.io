---
title: Eliminate INSERT locking in MySQL 5.1 with InnoDB
date: 2009-03-20 01:36:52 -04:00
permalink: "/2009-03-20/eliminate-insert-locking-in-mysql-51-with-innodb/"
categories:
- Notebook
tags:
- InnoDB
- Locking
- MySQL
- Performance
id: 93
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=93
---

To get the best locking performance out of InnoDB in 5.1, you&#8217;ll want to set the following options (&#8220;<a href="http://harrison-fisk.blogspot.com/2009/02/my-favorite-new-feature-of-mysql-51.html" target="_blank">My Favorite New Feature of MySQL 5.1: Less InnoDB Locking</a>&#8221; explains why):

`[mysqld]<br />
innodb_autoinc_lock_mode=2<br />
binlog_format=row<br />
tx_isolation=READ-COMMITTED`