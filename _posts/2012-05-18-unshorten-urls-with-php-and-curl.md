---
title: Unshorten URLs with PHP and cURL
date: 2012-05-18 12:12:01 -04:00
permalink: "/2012-05-18/unshorten-urls-with-php-and-curl/"
categories:
- Notebook
tags:
- bitly
- CURL
- PhantomJS
- PHP
id: 331
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=331
---

While working on a site that provides previews of URLs embedded in tweets using the awesome <a href="http://phantomjs.org/" target="_blank">PhantomJS</a> scriptable WebKit browser, and encountered difficulties when an URL shortener such as <a href="http://bit.ly" target="_blank">Bit.ly</a> was used (as is almost always done when tweeting out a link to an interesting article or photo).

After a little experimenting, I discovered that cURL makes it super easy to un-shorten a URL that has been shortened, without depending on a <a href="http://www.unshorten.com/" target="_blank">third</a>&#8211;<a href="http://unshort.me/" target="_blank">party</a> <a href="http://unshorten.it/" target="_blank">service</a>.

Improving slightly on <a href="http://codeaid.net/php/get-the-last-effective-url-from-a-series-of-redirects-for-the-given-url" target="_blank">this function</a>, here&#8217;s how I did it in PHP:

<pre>function unshorten_url($url) {
    $ch = curl_init($url);
    curl_setopt_array($ch, array(
        CURLOPT_FOLLOWLOCATION =&gt; TRUE,  // the magic sauce
        CURLOPT_RETURNTRANSFER =&gt; TRUE,
        CURLOPT_SSL_VERIFYHOST =&gt; FALSE, // suppress certain SSL errors
        CURLOPT_SSL_VERIFYPEER =&gt; FALSE, 
    ));
    curl_exec($ch);
    $url = curl_getinfo($ch, CURLINFO_EFFECTIVE_URL);
    curl_close($ch);
    return $url;
}</pre>

That&#8217;s all you have to do. This works even if multiple URL shorteners are used in sequence on the same link, and in the event of an error, you&#8217;ll just get the same link back that you started with.

This seems to be a bit more robust than some of the other solutions floating around the &#8216;net, since it doesn&#8217;t try to hack the <a href="http://stackoverflow.com/questions/7746852/how-can-i-unwrap-t-co-links-via-an-api-to-display-the-unshortened-url" target="_blank">response headers</a> or body (which could change without notice), and it will recurse as many times as it needs to up to the value of the <a href="http://us3.php.net/curl_setopt" target="_blank"><code>CURLOPT_MAXREDIRS</code></a> setting.

Enjoy!