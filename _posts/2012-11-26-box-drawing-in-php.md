---
title: Box drawing in PHP
date: 2012-11-26 11:20:08 -05:00
permalink: "/2012-11-26/box-drawing-in-php/"
categories:
- Notebook
tags:
- ANSI
- ASCII
- CLI
- PHP
- UTF-8
id: 364
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=364
---

 <img class="alignright" title="Code Page 437" src="http://upload.wikimedia.org/wikipedia/commons/f/f8/Codepage-437.png" alt="" width="304" height="144" />Back in the &#8220;good old days&#8221; of MS-DOS, you could draw lines, boxes, filled areas (think progress bars), and more using the extended ASCII character set (AKA <a href="http://en.wikipedia.org/wiki/Code_page_437" target="_blank">code page 437</a>).

While writing a simple command-line utility in PHP I wanted to use the full block (`█`) and light shade (`░`) characters to create a simple progress bar that is a bit nicer than the typical `=========...................`.

Like this:

<pre>█████████████████░░░░░░░░░░░░░░░░░░░░░░░ 42.5%</pre>

Instinctively, I turned to PHP&#8217;s <a href="http://php.net/chr" target="_blank"><code>chr()</code></a> function and <a title="ASCII table" href="http://www.asciitable.com/" target="_blank">looked up the extended ASCII codes</a> for the characters I needed. Boy, was I disappointed when my progress bar was nothing but a series of un-useful question marks. Surely PHP can render simple ASCII characters, I thought.

It might have gone differently if I had been using a PC, but I do 100% of my development these days on a Macbook Pro. It so happens that the bash shell in UNIX, Linux, and Mac OS X all use UTF-8 encoding by default, not CP437. (Of course, your terminal font will need to support UTF-8 characters for this to work.)

So, I just needed to find the UTF-8 codes for the characters I needed and use those instead of the old familiar CP437 ones. However, the results weren&#8217;t much better.

After an hour or more of Googling and experimentation, I finally realized that `chr()` doesn&#8217;t do UTF-8. I <a title="Stack Overflow: PHP construct a Unicode string?" href="http://stackoverflow.com/a/3704588" target="_blank">found various suggestions</a> on how to produce the desired characters using custom functions and the like, but the best way turned out to just use a good &#8216;ol HTML entity and run it through `html_entity_decode`:

<pre>$block = html_entity_decode('&#x2588;', ENT_NOQUOTES, 'UTF-8'); // full block
$shade = html_entity_decode('&#x2591;', ENT_NOQUOTES, 'UTF-8'); // light shade</pre>

Now, a progress bar isn&#8217;t exactly a box, so let&#8217;s demonstrate:

<pre>$tl = html_entity_decode('&#x2554;', ENT_NOQUOTES, 'UTF-8'); // top left corner
$tr = html_entity_decode('&#x2557;', ENT_NOQUOTES, 'UTF-8'); // top right corner
$bl = html_entity_decode('&#x255a;', ENT_NOQUOTES, 'UTF-8'); // bottom left corner
$br = html_entity_decode('&#x255d;', ENT_NOQUOTES, 'UTF-8'); // bottom right corner
$v = html_entity_decode('&#x2551;', ENT_NOQUOTES, 'UTF-8');  // vertical wall
$h = html_entity_decode('&#x2550;', ENT_NOQUOTES, 'UTF-8');  // horizontal wall

echo $tl . str_repeat($h, 15)  . $tr . "\n" .
     $v  . ' Hello, World! '   . $v  . "\n" .
     $bl . str_repeat($h, 15)  . $br . "\n";</pre>

Voilà!

<img class="alignnone" src="http://grab.by/hOiq" alt="" width="291" height="85" />

UTF-8, once you get used to it, actually <a title="Unicode Characters as Named and Numeric HTML Entities" href="http://la.remifa.so/unicode/unicode.php" target="_blank">provides many more possibilities</a> than CP437 did back in the MS-DOS days. Enjoy!

<img class="aligncenter" title="Code Page 437" src="http://upload.wikimedia.org/wikipedia/commons/8/85/Unicode_Box_Drawings_%282500_-_27FF%29.svg" alt="" width="420" />