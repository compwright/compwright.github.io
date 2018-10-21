---
title: Improving INSERT INTO … SELECT Performance
date: 2009-07-29 16:59:21 -04:00
permalink: "/2009-07-29/improving-insert-into-%e2%80%a6-select-performance/"
categories:
- Notebook
tags:
- Concurrency
- Drizzle
- MySQL
- Replication
id: 174
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=174
---

INSERT INTO&#8230;SELECT locks the table being read by the SELECT statement due to MySQL 5.0&#8217;s statement-based replication. Here&#8217;s a great post from the MySQL Performance Blog explaining the problem in detail and what to do about it.

Note: this problem is supposed to be eliminated when using MySQL 5.1 row-based replication.

I thought it would be interesting to see how the Drizzle developers where handling this scenario in their high-performance, high-concurrency remake of the MySQL. Below is an e-mail I got back from Jay Pipes on the subject:<!--more-->

> Jonathon Hill wrote:
> 
> > Hey guys,
> > 
> > I&#8217;m excited about Drizzle and look forward to its maturity!
> > 
> > I have just run into an issue with MySQL and was wondering how you might be handling this problem in Drizzle. Basically, when you do a INSERT INTO&#8230;SELECT query it has to lock the table being selected or replication doesn&#8217;t work right (see http://www.mysqlperformanceblog.com/2006/07/12/insert-into-select-performance-with-innodb-tables/). This is horrible for concurrence, because other clients are locked out until the SELECT is finished, which can be a while, depending on the query.
> 
> Not in Drizzle.
> 
> > MySQL 5.1 solves this with row-based replication. Will Drizzle support replication? If so, how are you planning to handle this scenario?
> 
> I just added a test case to the replication suite for INSERT SELECT. As you can see below, INSERT SELECT is translated in Drizzle to multiple InsertRecord GPB messages (kinda like row-based replication, but in a standardized serial protocol format).
> 
>     DROP TABLE IF EXISTS t1;
>     DROP TABLE IF EXISTS t2;
>     
>     CREATE TABLE t1 (
>        id INT NOT NULL, padding VARCHAR(200) NOT NULL
>     );
>     
>     INSERT INTO t1 VALUES (1, "I love testing.");
>     INSERT INTO t1 VALUES (2, "I hate testing.");
>     
>     CREATE TABLE t2 (
>        id INT NOT NULL, padding VARCHAR(200) NOT NULL
>     );
>     
>     INSERT INTO t2 SELECT * FROM t1;
> 
> Output from command log reader, which reconstructs the row-based events into SQL statements, with some header info removed:
  
> ``<br />
DROP TABLE IF EXISTS t1;<br />
DROP TABLE IF EXISTS t2;<br />
CREATE TABLE t1 ( id INT NOT NULL , padding VARCHAR(200) NOT NULL );<br />
INSERT INTO `test`.`t1` (`id`, `padding`) VALUES ("1", "I love testing.");<br />
INSERT INTO `test`.`t1` (`id`, `padding`) VALUES ("2", "I hate testing.");<br />
CREATE TABLE t2 ( id INT NOT NULL , padding VARCHAR(200) NOT NULL );<br />
INSERT INTO `test`.`t2` (`id`, `padding`) VALUES ("1", "I love testing.");<br />
INSERT INTO `test`.`t2` (`id`, `padding`) VALUES ("2", "I hate testing.");<br />
`` 
> 
> Cheers!
> 
> jay