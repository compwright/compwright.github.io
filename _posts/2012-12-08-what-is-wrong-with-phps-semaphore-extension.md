---
title: What Is Wrong With PHP&#8217;s Semaphore Extension
date: 2012-12-08 16:27:20 -05:00
permalink: "/2012-12-08/what-is-wrong-with-phps-semaphore-extension/"
categories:
- Notebook
tags:
- PHP
- Semaphore
id: 392
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=392
---

## Lack of a true Semaphore

> **sem_release()** releases the semaphore if it is currently acquired by the calling process, otherwise a warning is generated.
  
> <cite><a href="http://php.net/manual/en/function.sem-release.php" target="_blank">http://php.net/manual/en/function.sem-release.php</a></cite>

By far the worst, this limitation makes a whole class of problems much more difficult to solve, and in fact means that PHP&#8217;s Semaphore extension, despite the name, is really a <a title="Semaphore vs Mutex" href="http://en.wikipedia.org/wiki/Semaphore_(programming)#Semaphore_vs._mutex" target="_blank">mutex</a> (a semaphore with ownership, i.e., only the acquirer may release). <span style="line-height: 1.714285714; font-size: 1rem;">Furthermore, it is inconsistent with the behavior of equivalent functions in other languages such as C and Python.</span>

Please see <a href="https://github.com/compwright/phm/blob/master/src/phm/Lock/Semaphore.php" target="_blank">phm\Lock\Semaphore</a> for a kludgy workaround using mutexes, shared memory, and a message queue.<!--more-->

## Undefined error handling

What happens if you <a href="http://php.net/manual/en/function.sem-get.php" target="_blank">sem_get()</a> using a key that is already in use by a different type of resource, say, a System V shared memory segment? This is an issue because <a href="http://php.net/manual/en/function.ftok.php" target="_blank">ftok()</a> (the normal way of generating IPC keys for this stuff) is collision-prone, given a large enough filesystem.

## Undefined behavior of sem_get()

> [T]he documentation does not stipulate what will happen if the max\_acquire paramter is varied upon successive invocations of the sem\_get method. [S]o setting it to 100, then to 1 on 2 successive calls will have an undefined behavior.
  
> <cite><a href="http://php.net/manual/en/function.sem-get.php#76793" target="_blank">http://php.net/manual/en/function.sem-get.php#76793</a></cite>

This is avoidable, but it would be nice if this were fixed.

## Cannot disable semaphore auto-releasing

> If I write a program with a sem\_get and auto\_release=0, in case of shutdown, an other program should not be available to take the semaphore.
  
> <cite><a href="https://bugs.php.net/bug.php?id=52701" target="_blank">Bug #52701</a></cite>

## A semaphore may be deleted when other processes are waiting to acquire it

> sem\_remove() should take the SYSVSEM\_USAGE (see sysvsem.c) count into consideration when it is called. Only if this count is == 1 should the semaphore be removed. This will allow the last process that is using the semaphore to remove it from the system.
  
> <cite><a href="https://bugs.php.net/bug.php?id=44109" target="_blank">Bug #44109</a></cite>

## Conclusion

The mantra among the PHP community seems to be &#8220;less bitchin&#8217;, more fixin'&#8221;, which is admirable. I do not wish to criticize or complain, but rather to document the need and hopefully encourage someone who has the background in C development to fix this (alas, I do not have this).