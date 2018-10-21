---
title: Master-Master MySQL Replication&#8230;that hurts less
date: 2011-09-30 03:53:54 -04:00
permalink: "/2011-09-30/mysql-replication-that-hurts-less/"
categories:
- Notebook
tags:
- MySQL
- Replication
id: 288
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=288
---

If you have ever touched a MySQL slave, you know that they can and do frequently halt. While sync problems can be caused by many things—network outages, schema changes, etc—one of the most common problems in a dual-master setup is primary key collision.

## Primary Key Collision

&#8230;happens when records are added on two different servers to the same table and get the same `AUTO_INCREMENT` value. Fortunately, there is a trivially easy way to prevent this from happening.

### `auto-increment-increment=N`

Adding this to your `my.cnf` or `my.ini` file will make `AUTO_INCREMENT` increment by `N` rather than by 1. `N` is the number of replicated servers that are masters.

Combine this setting with&#8230;

### `auto-increment-offset=N`

&#8230;to ensure that each replicated master uses unique `AUTO_INCREMENT` values. `N` should match the `server-id` setting.

With these two settings in place, your primary keys will never collide.<!--more-->

## Example

Let&#8217;s suppose you have three replicated master MySQL servers, each with a `` `comments` `` table with a last id of 1000. Your servers are configured as follows:

### Server 1:

    server-id = 1
    auto-increment-increment = 3
    auto-increment-offset = 1

Server 1 will generate `AUTO_INCREMENT` values of the form `1 + 3N` (4, 7, 10, 13&#8230;), where `N` is a sequential positive integer.

### Server 2:

    server-id = 2
    auto-increment-increment = 3
    auto-increment-offset = 2

Server 2 will generate `AUTO_INCREMENT` values of the form `2 + 3N` (5, 8, 11, 14&#8230;), where `N` is a sequential positive integer.

### Server 3:

<pre>server-id = 3
auto-increment-increment = 3
auto-increment-offset = 3</pre>

Server 3 will generate `AUTO_INCREMENT` values of the form `3 + 3N` (6, 9, 12, 15&#8230;), where `N` is a sequential positive integer.

At the same instant, three different visitors to your web application comment. Here&#8217;s what happens to the `` `comments` `` table:

  * Server 1: `AUTO_INCREMENT` = 1003 (1 + 334 * 3)
  * Server 2: `AUTO_INCREMENT` = 1004 (2 + 334 * 3)
  * Server 3: `AUTO_INCREMENT` = 1005 (3 + 334 * 3)

Let&#8217;s suppose that replication stopped before these three new rows could be replicated on the other two servers, and three more visitors leave comments. What IDs will be assigned?

  * Server 1: `AUTO_INCREMENT` = 1006 (1 + 335 * 3)
  * Server 2: `AUTO_INCREMENT` = 1007 (2 + 335 * 3)
  * Server 3: `AUTO_INCREMENT` = 1008 (2 + 335 * 3)

Beautiful!