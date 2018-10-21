---
title: What to do when MySQL doesn&#8217;t use an index&#8230;
date: 2009-04-30 22:43:02 -04:00
permalink: "/2009-05-01/what-to-do-when-mysql-doesnt-use-an-index/"
categories:
- Notebook
tags:
- Index
- MySQL
- Optimization
- SQL
- Table scan
- UNION
id: 119
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=119
---

Sometimes <a href="http://dev.mysql.com/tech-resources/presentations/presentation-oscon2000-20000719/index.html" target="_blank">MySQL doesn&#8217;t use the index</a> on a column when performing a query.

> Indexes are NOT used if MySQL can calculate that it will probably be faster to scan the whole table. For example if `key_part1` is evenly distributed between 1 and 100, it&#8217;s not good to use an index in the following query:
> 
>   * `SELECT * FROM table_name where key_part1 > 1 and key_part1 < 90`

So what do you do in that case? This was my query:

<pre>SELECT dispId, outcome, contacted, householder, time_stamp
FROM `dispositions`
WHERE `leadId` = 1 OR (`leadId` IS NULL AND `campaignId` = 8 AND `xid` = 'AAA100000000148')
ORDER BY `dispId` DESC
LIMIT 1</pre>

In this table (~175,000 rows), leadId, campaignId, and xid all have indexes, but MySQL was doing a table scan (~2.2 sec) because of the duplicate use of leadId and the way the WHERE clause was structured. This rather unusual optimization was more than 10x faster (~.17 sec):

<pre>SELECT dispId, outcome, contacted, householder, time_stamp
FROM `dispositions`
WHERE `campaignId` = 8 AND `xid` = 'AAA100000000148' AND `leadId` IS NULL

UNION ALL

SELECT dispId, outcome, contacted, householder, time_stamp
FROM `dispositions`
WHERE `leadId` = 1

ORDER BY `dispId` DESC
LIMIT 1</pre>

An odd case where two queries are faster than one!