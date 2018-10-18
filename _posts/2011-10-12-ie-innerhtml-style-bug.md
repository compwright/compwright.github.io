---
id: 314
title: IE innerHTML CSS bug
date: 2011-10-12T15:51:22+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=314
permalink: /2011-10-12/ie-innerhtml-style-bug/
categories:
  - Notebook
tags:
  - bug
  - CSS
  - IE
  - Javascript
---
Today I encountered a bug that affects every version of Internet Explorer from IE6 all the way through IE9. Here&#8217;s what happens.

## The Bug

  1. Set a Javascript string variable containing CSS styles (`<style>` or `<link>`) followed by some HTML content, such as a `<p>` tag (in my application this happened via JSONP).
  2. Add a `<div>` node to the DOM
  3. Insert the content from #1 into the `.innerHTML` property of the `<div>`
  4. The content gets inserted, devoid of CSS styles.

    <!-- these styles are ignored when inserted via innerHTML -->
    <style>
      p { color: blue; }
    </style>
    <p>Hello, how are you!</p>

## The Solution

The <a title="IE not including CSS" href="http://www.houseoffusion.com/groups/ajax/thread.cfm/threadid:1304#4254" target="_blank">simple solution</a> was to put the other markup _before_ the `<style>` or `<link>` tag. Bizarre.

    <p>Hello, how are you!</p>
    <!-- these styles are applied when inserted via innerHTML -->
    <style>
     p { color: blue; }
    </style>

Because IE needs something that affects the layout before it will apply CSS styles in an innerHTML property, these will not work as style triggers:

  * `<!-- a comment -->`
  * `<p></p>` (or any other empty block-level HTML tag)