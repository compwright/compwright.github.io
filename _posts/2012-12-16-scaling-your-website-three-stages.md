---
id: 395
title: 'Scaling Your Website: Three Stages'
date: 2012-12-16T21:55:26+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=395
permalink: /2012-12-16/scaling-your-website-three-stages/
categories:
  - Notebook
tags:
  - Scaling
---
<span style="line-height: 1.714285714; font-size: 1rem;">So, you have A Big Idea that will Revolutionize The World. Naturally, people will flock to your site and engage with it. Naturally, you are concerned about whether your infrastructure can handle success. You wish to plan for success, it would be foolish not to, eh? </span><span style="line-height: 1.714285714; font-size: 1rem;">Don&#8217;t let enthusiasm (or ignorance) lead you to premature over-optimization. After all, your Big Idea has not yet passed the test of time.</span>

Here I will describe the three early stages commonly encountered when scaling a new website or startup application. Each one builds off the previous ones, starting with the lowest-hanging fruit and working up.<!--more-->

<span style="line-height: 1.714285714; font-size: 1rem;">Achieving each level requires significant amounts of work, testing, and benchmarking. Consider the economics and do not scale more than is necessary. A</span><span style="line-height: 1.714285714; font-size: 1rem;">s you grow, you will, if successful, have the revenue to scale with the demand.</span>

<span style="line-height: 1.714285714; font-size: 1rem;">At each stage you should benchmark to test the capacity you can handle, and monitor your load with respect to this benchmark so that you can scale proactively, before your site breaks rather than after.</span>

## Stage 1: (1) application server, (1) database server

  1. <span style="line-height: 1.714285714; font-size: 1rem;">Use an opcode cache such as </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://php.net/manual/en/book.apc.php" target="_blank">APC</a><span style="line-height: 1.714285714; font-size: 1rem;"> to speed up PHP execution.</span>
  2. <span style="line-height: 1.714285714; font-size: 1rem;">Move the database onto its own server.</span>
  3. <span style="line-height: 1.714285714; font-size: 1rem;">Run </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://www.percona.com/software/percona-server" target="_blank">Percona Server 5.5</a><span style="line-height: 1.714285714; font-size: 1rem;"> instead of MySQL 5.5.</span>
  4. <span style="line-height: 1.714285714; font-size: 1rem;">Tune your database server configuration (my.ini).</span>
  5. <span style="line-height: 1.714285714; font-size: 1rem;">Swap out </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://arstechnica.com/business/2011/11/a-faster-web-server-ripping-out-apache-for-nginx/" target="_blank">Apache for Nginx</a><span style="line-height: 1.714285714; font-size: 1rem;">, it is a lot more efficient.</span>
  6. <span style="line-height: 1.714285714; font-size: 1rem;">Store your </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://shiflett.org/articles/storing-sessions-in-a-database" target="_blank">user session data in the database</a><span style="line-height: 1.714285714; font-size: 1rem;"> (but watch out for </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://www.mysqlperformanceblog.com/2007/03/27/php-sessions-files-vs-database-based/" target="_blank">this problem</a><span style="line-height: 1.714285714; font-size: 1rem;">).</span>
  7. <span style="line-height: 1.714285714; font-size: 1rem;">Store static assets in the cloud and server them using a CDN (I recommend <a href="http://aws.amazon.com/s3/" target="_blank">Amazon S3</a> + <a href="http://aws.amazon.com/cloudfront/" target="_blank">Amazon CloudFront</a>, but there are others). This will take a large portion of the load off your application server.</span>
  8. <span style="line-height: 1.714285714; font-size: 1rem;">Store file uploads in the cloud (Amazon S3).</span>
  9. <span style="line-height: 1.714285714; font-size: 1rem;">Audit the database queries and table structures for inefficiencies (missing indexes, poorly optimized joins, excessive queries).</span>

Both servers should be running at least 512MB to 1GB of RAM. Properly configured, this simple setup will handle quite a beating. With fewer moving parts, you will have more time to develop and enhance your offering. As traffic grows, add RAM until it is more economical to move to Stage 2.

## Stage 2: (1-2) <a href="http://www.ha-cc.org/high_availability/components/application_availability/cluster/load_balancing_cluster/" target="_blank">load balancers</a>, (2-5) application servers, (1) database server

  1. <span style="line-height: 1.714285714; font-size: 1rem;">Add 1 or 2 dedicated servers with load-balancing software running.</span>
  2. <span style="line-height: 1.714285714; font-size: 1rem;">Add 2 or more identical copies of your application server.</span>
  3. <span style="line-height: 1.714285714; font-size: 1rem;">Scale up your database server to at least 2GB RAM and re-tune your database server configuration.</span>
  4. <span style="line-height: 1.714285714; font-size: 1rem;">Change your domain DNS to point to the IP address of the load balancer.</span>
  5. <span style="line-height: 1.714285714; font-size: 1rem;">Use a deployment method that supports server clusters, such as </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://capistranorb.com/" target="_blank">Capistrano</a>.

### Cloud-hosted load balancing

<a href="http://www.rackspace.com/cloud/public/loadbalancers/" target="_blank">Rackspace</a> and <a href="http://aws.amazon.com/elasticloadbalancing/" target="_blank">Amazon</a> provide load balancing as a service. You get redundancy, and you can use smaller web servers (256-512MB RAM) for cost savings. Amazon provides more datacenters and more flexibility. The cost of rolling your own load balancer is unlikely to be justified at this stage, and I highly recommend using one of these unless you have skilled system administrators on staff and have done a cost analysis that favors self-hosting.

When combined with Amazon&#8217;s <a href="http://aws.amazon.com/route53/" target="_blank">Route 53 DNS service</a>, you can configure multiple load balancers in multiple regions to provide an even greater level of redundancy.

### Rolling your own load balancer

There are several open-source load balancing applications available, evaluate and choose the best one for your needs:

  * Squid Cache, <a href="http://wiki.squid-cache.org/ConfigExamples#Reverse_Proxy_.28Acceleration.29" target="_blank">configured as a reverse proxy</a>. Note that Squid can handle SSL (https) if needed, allowing your application servers to communicate with the load balancer more quickly over normal HTTP.
  * <a href="http://haproxy.1wt.eu/" target="_blank">HAProxy</a>
  * Nginx, <a href="http://tumblr.intranation.com/post/766288369/using-nginx-reverse-proxy" target="_blank">configured as a reverse proxy</a>
  * <a href="http://www.apsis.ch/pound" target="_blank">Pound</a>

If you are running more than one load balancer, use <a href="http://www.keepalived.org/" target="_blank">keepalived</a> to handle failover.

### Code deployments

When multiple application servers are involved, you will need an automated way to push out code deployments. There are several ways to do this:

  * Create a tool using the <a href="http://capistranorb.com/" target="_blank">Capistrano</a> Ruby gem
  * Roll your own tool using your preferred scripting language and rsync
  * Configure a protected git server with your code, and set up a cronjob to perform a \`git pull\`
  * Use a hosted code repository service like <a href="http://beanstalkapp.com/" target="_blank">Beanstalk</a> which supports code deployment via SFTP

## Stage 3: (2+) load balancers, (5-10) application servers, (1) master database server, (2+) slave database servers

  1. <span style="line-height: 1.714285714; font-size: 1rem;">Scale your primary (master) database server to a sufficiently large size. Use </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://aws.amazon.com/rds/" target="_blank">Amazon RDS</a> <span style="line-height: 1.714285714; font-size: 1rem;">for a cloud-based solution, or get a high-performance dedicated server with 4-8 CPU cores, at least 3 SSD hard drives in RAID 5, and 8-32GB RAM (use at least as much RAM as your database size). All database inserts, updates, and deletes will be done on this server.</span>
  2. <span style="line-height: 1.714285714; font-size: 1rem;">Add two or more dedicated database servers to function as read replicas. The server specifications will need to match that of your master database server, despite the fact they will be doing nothing but handling database read requests. Configure MySQL master-slave replication or </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://www.drbd.org/" target="_blank">DRBD</a><span style="line-height: 1.714285714; font-size: 1rem;"> to keep your read replicas in sync with the master database.</span>
  3. <span style="line-height: 1.714285714; font-size: 1rem;">Update your application code to connect to the master database for writes and to the replicas for reads. Using a random or round robin scheme for choosing a read replica to connect to, or some other method based on your needs.</span>

## Beyond Stage 3

Beyond this point, scaling your site and keeping it running smoothly will require a dedicated _team_ of system administrators, architects, developers, and testers. This is the situation that large-scale websites like Facebook, Twitter, Pinterest, and Etsy currently have and it requires lots of innovation, time, and cash.

## Specialized Tools/Techniques

These could be selectively intermingled anywhere along the way, as needed.

  * Search &#8211; use a dedicated search server, such as <a href="http://sphinxsearch.com/" target="_blank">Sphinx</a>.
  * Use <a href="http://memcached.org/" target="_blank">memcached</a> on dedicated servers (or <a href="http://aws.amazon.com/elasticache/" target="_blank">Amazon ElastiCache</a>) to cache parts of your application, and/or your user session data.
  * Migrate part or all of your database to a non-relational database, such as MongoDB (or <a href="http://aws.amazon.com/dynamodb/" target="_blank">Amazon DynamoDB</a>).
  * Partition or shard very large databases to get less-frequently-accessed data out of the way (and out of your database cache).
  * Implement <a href="http://vimeo.com/53746372#at=0" target="_blank">load balanced work queues</a> using <a href="http://www.gearman.org/gearman" target="_blank">Gearman</a> or similar to do background processing in an asynchronous fashion.
  * Run reporting database queries on a separate database. Either run your reports on a dedicated read replica, or use one of the specialized data warehouse solutions that are on the market (such as <a href="http://www.infobright.com/" target="_blank">Infobright</a>).

## Conclusion

Evaluate how much you really need to scale. The decision needs to be based on the data, of how much traffic you actually have now, how much you can afford to spend on scaling, and how much traffic you reasonably anticipate in the short term future&#8211;not on technological fads or overly optimistic dreams of grandeur. Remember that <a href="http://c2.com/cgi/wiki?PrematureOptimization" target="_blank">premature optimization will get you in trouble</a>.

Should you do this yourself, or hire consultants? That depends on how fast you anticipate growing and what you can afford to pay. In light of the cost of hiring and retaining really competent developers and system administrators, you may find <a href="http://www.mysqlperformanceblog.com/2008/03/13/economics-of-performance-optimization/" target="_blank">using consultants in the early stages to be more cost effective</a>.