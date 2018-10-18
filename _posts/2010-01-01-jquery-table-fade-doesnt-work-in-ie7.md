---
id: 217
title: 'jQuery table fade doesn&#8217;t work in IE7'
date: 2010-01-01T21:19:41+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=217
permalink: /2010-01-01/jquery-table-fade-doesnt-work-in-ie7/
categories:
  - Notebook
tags:
  - effects
  - IE7
  - jQuery
  - opacity
---
While working on an up-and-coming web service, I found that apparently Internet Explorer does not cope well with fading `<table>` elements using [jQuery](http://jquery.com). Here&#8217;s what I was doing:

<pre name="code" class="javascript">tbl = $('#primaryColumn table');
loading = $('#primaryColumn .loading');

tbl.fadeTo(300, 0.0, function() {
    loading.show();
        tbl.load('/contacts/{pagination:page}/' + page_num + '?ajax&search={pagination:search}', function() {
        loading.hide();
        tbl.stop().fadeTo(300, 1.0, function() {
            tbl.css('opacity', 'auto');  // removing the CSS opacity rule restores the ClearType anti-aliasing in IE
        });
    });
});</pre>

For some reason, the `fadeIn()`, `fadeOut()`, and `fadeTo()` effects do not work on `<table>` elements in IE, although they work great in Firefox and Opera. This also applies when using `animate()` to alter the CSS `opacity` rule (yeah, I tried them all).

As usual, the solution proved to be very easy: don&#8217;t animate the `<table>`, rather, wrap the `<table>` in a `<div>` and animate that. The only change required is the first line:

<pre name="code" class="javascript">tbl = $('#primaryColumn div#table_div');</pre>

Bingo! Another three-hour bug-swatting episode reinforcing my hatred for Microsoft browsers of all versions just concluded.