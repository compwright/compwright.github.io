---
id: 248
title: Catching the Javascript beforeunload event, the cross-browser way
date: 2011-03-04T11:36:40+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=248
permalink: /2011-03-04/catching-the-javascript-beforeunload-event-the-cross-browser-way/
categories:
  - Notebook
tags:
  - beforeunload
  - Javascript
  - jQuery
  - onbeforeunload
---
Javascript&#8217;s `window.onbeforeunload` event is useful for catching users that try to browse away from your page without having completed a certain action. Modern web applications such as <a href="http://gmail.com" target="_blank">Gmail</a> and <a href="http://wordpress.org" target="_blank">WordPress</a> have made good use of this event.

Being a non-standard event originally invented by Microsoft back in the IE4 days, `window.onbeforeunload` has some real quirks, although thankfully every major modern browser does support it.

## jQuery Doesn&#8217;t Help

Prior to jQuery 4, you couldn&#8217;t even bind to `$(window).bind('beforeunload')` due to a <a href="http://bugs.jquery.com/ticket/4106" target="_blank">bug</a> that has been fixed.

However, this isn&#8217;t your average Javascript event. `window.onbeforeunload` pops up a native dialog box that provides very little opportunity for customization beyond the text that is displayed to the user. There is no known way to disable this native dialog box and prevent normal behavior.

Tapping into jQuery&#8217;s `$(window).unload()` event doesn&#8217;t allow you to prevent the page from being unloaded, and I couldn&#8217;t get `$(window).bind('beforeunload')` to work at all in Firefox 3.6.

## The Right Way

The right way turned out to be quite easy using native Javascript code (thanks to the <a href="https://developer.mozilla.org/en/DOM/window.onbeforeunload" target="_blank">Mozilla Doc Center</a> for the working solution).

For IE and FF < 4, set `window.event.returnValue` to the string you wish to display, and then return it for Safari (use `null` instead to allow normal behavior):

    window.onbeforeunload = function (e) {
        var e = e || window.event;
    
        // For IE and Firefox prior to version 4
        if (e) {
            e.returnValue = 'Any string';
        }
    
        // For Safari
        return 'Any string';
    };