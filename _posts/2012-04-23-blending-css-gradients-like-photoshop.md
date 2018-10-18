---
id: 325
title: Blending CSS Gradients Like Photoshop
date: 2012-04-23T13:26:09+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=325
permalink: /2012-04-23/blending-css-gradients-like-photoshop/
categories:
  - Notebook
tags:
  - CSS
  - CSS3
  - Gradient
  - Photoshop
---
While on my day job over at <a href="http://company52.com" target="_blank">Company 52</a>, I encountered a textured background with a gradient overlay, using Photoshop&#8217;s **overlay** blending mode. I&#8217;m sure you&#8217;ve seen this effect before:

<div style="height: 100px; background: url('/wp-content/uploads/2012/css-gradient-blending-demo/photoshop-gradient-overlay.png') top center no-repeat;">
</div>

&nbsp;

My first thought was to save the texture as a 4&#215;4 PNG for tiling, and to save the gradient as a PNG with alpha transparency to overlay over the tiled pattern. This would certainly be lighter than saving a monolithic image, but this presented several problems:

  1. The size of the gradient PNG was still too large, especially when the gradient can be done in CSS
  2. The gradient overlay would be overlaid like Photoshop&#8217;s **normal** blending mode, not in the desired **overlay** mode, which would make the background look washed out.

<a title="Adobe: Bringing blending to the Web" href="http://blogs.adobe.com/webplatform/2012/04/04/bringing-blending-to-the-web/" target="_blank">CSS blending modes</a> would solve this, but they aren&#8217;t here yet (<a title="StackOverflow: Are photoshop-like blend modes possible in HTML5?" href="http://stackoverflow.com/questions/4276859/are-photoshop-like-blend-modes-possible-in-html5" target="_blank">you can do it with the HTML5 Canvas API</a>, but that&#8217;s messy).<!--more-->

## A Layered Approach

On a close examination of the pattern, you can see that the gradient only affects the lighter dots, suggesting a layered approach to _simulate_ the desired affect:

  1. Top layer: create a new tiled background image like the original one, but with the lighter dots which should be affected by the gradient overlay erased, leaving only the darker ones.
  2. Middle layer: the desired CSS gradient.
  3. Bottom layer: the original background image.

<img class="alignnone" title="Layer illustration" src="/wp-content/uploads/2012/css-gradient-blending-demo/layers.png" alt="" style="max-width:100%" />

## Implementation

My new foreground and background tiles ended up looking like this:<figure style="width: 457px" class="wp-caption alignnone">

<img title="Screenshot of tile images in Photoshop" src="/wp-content/uploads/2012/css-gradient-blending-demo/sample-psd.png" alt="" width="457" height="358" /><figcaption class="wp-caption-text">Left side: foreground tile; Right side: background tile</figcaption></figure> 

As you can see, the foreground image acts as a screen which overlays the CSS gradient, restoring the desired contrast of our background much like Photoshop&#8217;s **overlay** blending mode.

Assembling the layers was easy with CSS3 multiple background images (vendor gradient prefixes are omitted for clarity):

    background-color: rgba(128,128,128,.05);
    background-image: url(background.png);
    background-image: url(foreground.png), radial-gradient(center top, circle closest-corner, rgba(255,255,255,.25) 0%, rgba(0,0,0,.1) 80%), url(background.png);
    background-position: top left, center center, top left;
    background-repeat: repeat, no-repeat, repeat;

<a href="http://ie.microsoft.com/testdrive/Graphics/CSSGradientBackgroundMaker/Default.html" target="_blank">This gradient CSS generator</a> was very helpful.

Older browsers will gracefully degrade to the original background image.

## Demo

<a href="/wp-content/uploads/2012/css-gradient-blending-demo/demo.html" target="_blank">Click here to see the demo</a>.

Voila! A nice Photoshop gradient overlay effect, implemented in an extremely lightweight manner with two 150-byte 4&#215;4 PNG images, and a CSS gradient.