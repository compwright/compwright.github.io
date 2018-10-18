---
id: 436
title: Deep linking into an iframe, cross-domain
date: 2013-10-22T12:14:01+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=436
permalink: /2013-10-22/deep-linking-into-an-iframe-cross-domain/
categories:
  - Notebook
tags:
  - Javascript
---
Why would you want to do this? My use case was a gallery application where we needed to deep-link to a specific gallery entry. Alas, the gallery would be iframed. Yes, iframes <a href="http://stackoverflow.com/questions/1081315/why-developers-hate-iframes" target="_blank">should be avoided</a>, but sometimes in real-life you have to just deal with it.

Imagine my surprise when I discovered that passing the parent `window.location` object into an iframe across domains is not only possible, but is easy and <a href="http://caniuse.com/x-doc-messaging" rel="nofollow">works in most browsers</a>, down to IE8.<!--more-->

## The Solution

Use `window.postMessage()` to pass messages to the iframed window.

## <strong style="line-height: 1.714285714; font-size: 1rem;">Outer page</strong>

    window.onload = function()
    {
        var child = document.getElementById('deep_link_frame');
        var msg   = {
            "location" : {
                "hash"     : window.location.hash,
                "host"     : window.location.host,
                "hostname" : window.location.hostname,
                "href"     : window.location.href,
                "origin"   : window.location.origin,
                "pathname" : window.location.pathname,
                "port"     : window.location.port,
                "protocol" : window.location.protocol,
                "search"   : window.location.search
            }
        };
        child.contentWindow.postMessage(JSON.stringify(msg), '*');
    };

**Inner page**

    function bindEvent(el, eventName, eventHandler)
    {
        if (el.addEventListener)
        {
            el.addEventListener(eventName, eventHandler);
        }
        else
        {
            el.attachEvent('on' + eventName, eventHandler);
        }
    }
    
    bindEvent(window, 'message', function(e)
    {
        if (e.origin === "http://your-domain.com")
        {
            var message = JSON.parse(e.data);
            alert(message.location.href);
        }
    });

## Limitations

There are a few land mines to watch out for if you need to support IE8 and IE9:

  * All messages should be sent as strings to avoid a nasty bug in IE8-9. If you want to pass an object, pass it in JSON format.
  * You can&#8217;t `JSON.serialize()` the `window.location` object in IE8. If you are trying to pass that object, you have to copy the properties one by one.
  * IE only supports `el.contentWindow.postMessage()`, not `el.postMessage()`.