---
title: Rules for Practical Programmers
date: 2009-03-10 13:39:28 -04:00
permalink: "/2009-03-10/rules-for-practical-programmers/"
categories:
- Notebook
tags:
- Business
- Frameworks
- Patterns
- Philosophy
- Syntax
id: 84
author: Jonathon
layout: post
guid: http://jonathonhill.net/2009-03-10/rules-for-practical-programmers/
---

Here are some observations I&#8217;ve gathered from writing a variety of programs in PHP over the last year. Do you have one I should add? Please share it and post it in the comments.

### 1) Don&#8217;t waste time re-inventing the wheel unless you can make a significantly better one.

Custom frameworks have their advantages, but are really necessary only if you are developing a particularly unusual project or actually have time to invest in writing framework. There are several great frameworks out there which are mature, tested, and have communities, all time-saving assets which no business-smart programmer should overlook.

Can&#8217;t find a framework that suits perfectly? Consider forking one.

### 2) Pretty code syntax should not be more important than re-usable code.

Case in point: static class methods nice and easy to use, but only if you don&#8217;t ever need to extend them (that is, until PHP 5.3 is production-ready). Make code decisions on the basis of functionality and intended use rather than &#8220;prettiness.&#8221;

### 3) Learning to use design patterns will save time in the long run, but don&#8217;t overdo it.

Remember the law of diminishing returns.

### 4) Comment religiously, and make your _code_ easy to read, too.

This involves descriptive variable names, following code style conventions, etc. Make comments useful: &#8220;# Assign the variable&#8221; tells me nothing I can&#8217;t already see.