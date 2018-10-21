---
title: Lone Star PHP takeaways
date: 2012-06-30 13:19:32 -04:00
permalink: "/2012-06-30/lone-star-php-takeaways/"
categories:
- Notebook
tags:
- GD
- ImageMagick
- IMagick
- LoneStarPHP
- OOP
- PECL
- Performance
- PHP
- phpunit
- Refactoring
- SOLID
id: 343
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=343
---

Thanks to the guys at <a href="http://www.engineyard.com/" target="_blank">Engine Yard</a>, I won a free pass to the <a href="http://lonestarphp.com" target="_blank">Lone Star PHP</a> <a href="http://eventifier.co/event/lsp12" target="_blank">community conference</a> in Dallas, TX this year. Due to the choice of subject matter and presenters, I think I enjoyed this conference even more than [last year&#8217;s PHPCon](http://jonathonhill.net/2011-04-21/phpcon-day-1/) in Nashville. Here are some of the highlights.

## Day 1

### !Normal===Awesome!

<a href="http://calevans.com/" target="_blank">Cal Evans</a> inspired us all to go out and build awesome stuff, and that we owe a debt to the giants who came before us and gave, to give to those coming after.

  * &#8220;This stuff is hard. If it looks easy, it&#8217;s because I&#8217;m good at it.&#8221;
  * Get involved somewhere.
  * Be a builder, a speaker, a teacher.

### Building Testable PHP Applications

<a href="http://grumpy-testing.com/" target="_blank">Chris Hartjes</a> delivered a really helpful talk on testing applications. One of the big obstacles of unit testing for me was mock objects. Chris did a good job explaining how they are used in the context of PHPUnit. Key takeaways for me:

  * <span style="text-decoration: underline;">Automate test execution.</span>
  * Following the <a href="http://en.wikipedia.org/wiki/Law_of_Demeter" target="_blank">law of demeter</a> will make your applications easier to test.
  * Code in a framework-agnostic and environment-agnostic way.
  * PHPUnit provides methods for setting up mock objects, so you don&#8217;t have to actually define those classes.
  * Test private/protected methods and properties by using a <a href="http://www.gpug.ca/2012/06/02/testing-protected-methods-with-phpunit/" target="_blank">Reflection hack</a>. <a href="https://github.com/etsy/phpunit-extensions" target="_blank">Etsy&#8217;s PHPUnit extensions</a> can automate this.
  * Don&#8217;t access the database in Unit tests, use mock objects instead. Filesystem access can be <a href="http://tech.vg.no/2011/03/09/mocking-the-file-system-using-phpunit-and-vfsstream/" target="_blank">mocked using vfsStream</a>.
  * Some applications resist testing, no matter what. If you cannot refactor, you can still automate tests with <a href="http://behat.org/" target="_blank">Behat</a>.
  * If you don&#8217;t test, Chris will cut you!

<a href="https://speakerdeck.com/u/grumpycanuck/p/building-testable-php-applications" target="_blank">See his slides</a> at SpeakerDeck.com.

### PHP Extensions for Dummies

<a href="http://emsmith.net/" target="_blank">Elizabeth Smith</a> laid bare the innards of PHP in her intro to PHP extension development. Much of it was unintelligible to me since I don&#8217;t (yet) know C. However, now I know who to go to if I need help compiling an extension! If I can navigate the dependencies, I&#8217;d like to try compiling a non-thread-safe Windows build of php_wkhtmltox for a future blog post.

<div id="__ss_13500035" style="width: 425px;">
  <strong style="display: block; margin: 12px 0 4px;"><a title="Php Extensions for Dummies" href="http://www.slideshare.net/auroraeosrose/php-extensions-elizabeths-mac-book-air" target="_blank">Php Extensions for Dummies</a></strong> </p> 
  
  <div style="padding: 5px 0 12px;">
    View more <a href="http://www.slideshare.net/thecroaker/death-by-powerpoint" target="_blank">PowerPoint</a> from <a href="http://www.slideshare.net/auroraeosrose" target="_blank">Elizabeth Smith</a>
  </div>
</div>

### Pixel Punching with PHP

<a href="http://catch404.net/" target="_blank">Bob Majdak</a>&#8216;s presentation compared the usage and performance of GD2 and IMagick across a number of common image manipulation tasks. IMagick is better in almost every way.

<a href="https://speakerdeck.com/u/bobmajdakjr/p/pixel-punching-with-php" target="_blank">See his slides</a> at SpeakerDeck.com.

## Day 2

### It Was Like That When I Got Here: Steps Toward Modernizing a Legacy Codebase

<a href="http://paul-m-jones.com" target="_blank">Paul Jones</a>&#8216; keynote did not disappoint. With insight and humor, Paul dismantled the problems with legacy codebases and presented a logical methodology to improving your life when you are forced to inherit a big stinkin&#8217; pile of spagetti code.

  * No matter how bad you want to, resist the urge to throw it out and rewrite.
  * You&#8217;ve nothing to lose. Either bear the pain of bad code, or bear the pain of refactoring bad code. May as well put the suffering to some use.
  * Move class files to a common base directory and autoload, removing require/include calls as you go.
  * Change class names as you go, if needed.
  * Wrap procedural functions in a class, so that it can be autoloaded.
  * Replace globals incrementally, moving them first to the constructor, then replacing them with dependency injection

<div style="width:425px" id="__ss_13505608">
  <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/pmjones88/it-was-like-that-when-i-got-here" title="It Was Like That When I Got Here" target="_blank">It Was Like That When I Got Here</a></strong> </p> 
  
  <div style="padding:5px 0 12px">
    View more <a href="http://www.slideshare.net/" target="_blank">presentations</a> from <a href="http://www.slideshare.net/pmjones88" target="_blank">pmjones88</a>
  </div></p>
</div>

### SOLID &#8211; Not Just a State Of Matter, It&#8217;s Principles for OO Propriety

<a href="http://www.chrisweldon.net/" target="_blank">Chris Weldon</a> laid out five principles to get the most out of object oriented development.

  * <span style="text-decoration: underline;"><strong>S</strong></span>ingle responsibility: each class should do one and only one thing.
  * <span style="text-decoration: underline;"><strong>O</strong></span>pen/Closed principle: each class is open to extension, but closed for modification. Don&#8217;t break stuff.
  * <span style="text-decoration: underline;"><strong>L</strong></span>iskov Substitution principle: replace if&#8230;else constructs and prevent exceptions by using typed collections.
  * <span style="text-decoration: underline;"><strong>I</strong></span>nterface segregation: each interface should be as focused as possible, so that it can be used by as many classes as need it. Similar to the Open/Closed principle, applied to interfaces.
  * <span style="text-decoration: underline;"><strong>D</strong></span>ependency Inversion: injection dependencies from the outside, don&#8217;t couple on the inside.

### Fast, Not Furious: Performance Tuning that Works

<a href="http://daveyshafik.com/" target="_blank">Davey Shafik</a> demonstrated how to use <a href="http://pecl.php.net/package/xhprof" target="_blank">xhprof</a>/<a href="https://github.com/preinheimer/xhprof" target="_blank">xh-gui</a> and <a href="http://memcached.org/" target="_blank">Memcache</a> to optimize your PHP application.

  * 1. Benchmark. 2. Profile. 3. Change.
  * Profiling affects performance, similar to quantum observation.
  * Benchmarking tests actual runtime, from start to end.
  * Namespacing Memcache keys can help overcome the 1MB size limit
  * Create new namespaces, don&#8217;t clear the cache. Garbage collection will clean up the old unused namespaces.
  * &#8220;Hardware is cheap, programmers are expensive&#8221; only holds when you take care of your hardware.
  * Don&#8217;t make assumptions, test everything sequentially.