---
title: Three ways to tell if your are running PHP 5.3
date: 2010-12-01 06:28:35 -05:00
permalink: "/2010-12-01/how-to-tell-if-your-are-running-php-5-3/"
categories:
- Notebook
tags:
- PHP
- PHP 5.3
- PHP_VERSION
id: 240
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=240
---

## A quick-n-dirty way

If all you want to do is see if you are running PHP 5.3+, then just check for the existence of the <a href="http://php.net/array_replace" target="_blank"><code>array_replace()</code></a> function, which was added in PHP 5.3:

<pre class="php"><code>&lt;?php
if(function_exists('array_replace')) {
    // running PHP 5.3+
} else {
    // running something prior to PHP 5.3
}
?&gt;</code></pre>

## The &#8220;right way&#8221;

The <a href="http://www.php.net/manual/en/function.version-compare.php" target="_blank"><code>version_compare()</code></a> method can be used in conjunction with the `PHP_VERSION` constant to compare standardized PHP version strings. It also makes for more readable code:

<pre class="php"><code>if(version_compare(PHP_VERSION, '5.3.0') &gt;= 0) {
    echo 'I am at least PHP version 5.3.0, my version: ' . PHP_VERSION . "\n";
}
</code></pre>

## Version constants

If you need to get down to the nitty gritty specifics of the PHP version you are running, use the `PHP_VERSION_ID`, `PHP_MAJOR_VERSION`, `PHP_MINOR_VERSION`, and `PHP_RELEASE_VERSION` constants, which were <a href="http://www.php.net/manual/en/reserved.constants.php#reserved.constants.core" target="_blank">added in PHP 5.2.7</a>. To ensure backward compatibility, the following <a href="http://php.net/manual/en/function.phpversion.php#Examples" target="_blank">code snippet from php.net</a> will define these constants if they are undefined in your PHP version:

<pre class="php"><code>&lt;?php
// PHP_VERSION_ID is available as of PHP 5.2.7, if our
// version is lower than that, then emulate it
if (!defined('PHP_VERSION_ID')) {
    $version = explode('.', PHP_VERSION);
    define('PHP_VERSION_ID', ($version[0] * 10000 + $version[1] * 100 + $version[2]));
}

// PHP_VERSION_ID is defined as a number, where the higher the number
// is, the newer a PHP version is used. It's defined as used in the above
// expression:
//
// $version_id = $major_version * 10000 + $minor_version * 100 + $release_version;
//
// Now with PHP_VERSION_ID we can check for features this PHP version
// may have, this doesn't require to use version_compare() everytime
// you check if the current PHP version may not support a feature.
//
// For example, we may here define the PHP_VERSION_* constants thats
// not available in versions prior to 5.2.7

if (PHP_VERSION_ID &lt; 50207) {
    define('PHP_MAJOR_VERSION',   $version[0]);
    define('PHP_MINOR_VERSION',   $version[1]);
    define('PHP_RELEASE_VERSION', $version[2]);
    // and so on, ...
}
?&gt;</code></pre>