---
title: Installing the CodeIgniter plugin for PHP_CodeSniffer
date: 2013-09-11 02:49:13 -04:00
permalink: "/2013-09-11/installing-the-codeigniter-plugin-for-php_codesniffer/"
categories:
- Notebook
tags:
- homebrew
- IDE
- PEAR
- php54
- phpcs
- PHP_CodeSniffer
- Static Analysis
- Sublime
id: 430
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=430
---

Since reading Phil Sturgeon&#8217;s post on <a href="http://philsturgeon.co.uk/blog/2013/08/php-static-analysis-in-sublime-text" target="_blank">PHP Static Analysis in the Sublime text editor</a>, I have been experimenting with using <a href="http://pear.php.net/package/PHP_CodeSniffer/" target="_blank">phpcs</a> and <a href="http://www.sublimetext.com/" target="_blank">Sublime</a> in general. Since I am currently used to the <a href="http://ellislab.com/codeigniter/user-guide/general/styleguide.html" target="_blank">CodeIgniter coding standard</a>, the time finally came today to try and configure my setup for that standard instead of <a href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md" target="_blank">PSR-2</a>.

_(A downgrade? Perhaps, but it&#8217;s what we use at work and it fits better into my workflow at the moment. Besides, the PSR-2 bracket rules annoy me to no end.)_

After installing phpcs on top of my <a href="http://brew.sh/" target="_blank">homebrewed</a> <a href="https://github.com/josegonzalez/homebrew-php" target="_blank">php54</a> installation, <a href="https://github.com/thomas-ernest/CodeIgniter-for-PHP_CodeSniffer" target="_blank">adding the CodeIgniter standard to PHP_CodeSniffer</a> was a little more tricky than expected. The catch is to specify the correct path to the PEAR module in the ant build script, which is NOT the same thing as the path to the phpcs command:

<pre>sudo ant -Dphpcs.dir=/usr/local/Cellar/php54/5.4.19/lib/php/PHP/CodeSniffer/</pre>

After that, we&#8217;re off to the races. Verifying that the install works:

<pre>$ /usr/local/Cellar/php54/5.4.19/bin/phpcs -iThe installed coding standards are CodeIgniter, MySource, PEAR, PHPCS, PSR1, PSR2, Squiz and Zend</pre>

Great! Now just change the phpcs settings in the Sublime PHP Code Sniffer config file to CodeIgniter, and we&#8217;re done.

<pre>"phpcs_additional_args": {
 "--standard": "CodeIgniter",
 "-n": ""
 },</pre>