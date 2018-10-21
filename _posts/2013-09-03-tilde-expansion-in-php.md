---
title: Tilde expansion in PHP
date: 2013-09-03 06:00:38 -04:00
permalink: "/2013-09-03/tilde-expansion-in-php/"
categories:
- Notebook
tags:
- CLI
- PHP
- POSIX
- tilde
- UNIX
id: 420
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=420
---

If you have ever written a PHP command line script and tried to pass a file to it using the POSIX tilde (`~`) shortcut to reference your home directory, you may have been surprised to learn that the operating system does not automatically expand tildes in paths.

You&#8217;ll get an error like this:

    Warning: fopen(~/file.csv): failed to open stream: No such file or directory

Fortunately, it is an easy matter to perform tilde expansion, once you know you need to.

    function expand_tilde($path)
    {
        if (function_exists('posix_getuid') && strpos($path, '~') !== false) {
            $info = posix_getpwuid(posix_getuid());
            $path = str_replace('~', $info['dir'], $path);
        }
    
        return $path;
    }

H/T to <a title="Stack Overflow: chdir() to home directory" href="http://stackoverflow.com/a/9493241/168815" target="_blank">Ernest Friedman-Hill</a> for cluing me into the solution.