---
id: 207
title: 'Javascript reserved words trigger &#8220;Expected Identifier&#8221; error on IE'
date: 2009-11-24T07:47:27+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=207
permalink: /2009-11-24/javascript-reserved-words-trigger-expected-identifier-error-on-ie/
categories:
  - Notebook
tags:
  - IE
  - Javascript
---
If you&#8217;re getting an unreasonable &#8220;Expected Identifier&#8221; Javascript error on IE6/7, check to see if you have any variable names which are reserved words.

This also goes for HTML form element names:

    <form name="aform">
        <input type="text" name="name" />
    </form>

Then accessing `document.aform.name.value` would throw an error since _name_ is a reserved word.

## Javascript Reserved Words

  * abstract
  * alert
  * Anchor
  * Area
  * arguments
  * Array
  * assign
  * blur
  * boolean or Boolean
  * break
  * Button
  * byte
  * callee
  * caller
  * captureEvents
  * **case**
  * catch
  * char
  * Checkbox
  * **class**
  * clearInterval
  * clearTimeout
  * close
  * **closed**
  * **comment**
  * **confirm**
  * const
  * constructor
  * continue
  * Date
  * debugger
  * **default**
  * defaultStatus
  * delete
  * do
  * document
  * Document
  * double
  * Element
  * else
  * enum
  * **escape**
  * eval
  * **export**
  * extends
  * false
  * FileUpload
  * final
  * finally
  * find
  * float
  * focus
  * for
  * Form
  * Frame
  * frames
  * function
  * Function
  * getClass
  * goto
  * Hidden
  * history or History
  * home
  * if
  * **Image**
  * implements
  * import
  * in
  * Infinity
  * innerHeight
  * innerWidth
  * instanceOf
  * int
  * interface
  * isFinite
  * isNan
  * java
  * JavaArray
  * JavaClass
  * JavaObject
  * JavaPackage
  * **label**
  * **length**
  * Link
  * **location** or Location
  * locationbar
  * long
  * Math
  * menubar
  * MimeType
  * moveBy
  * moveTo
  * **name**
  * NaN
  * native
  * navigate
  * navigator or Navigator
  * netscape
  * new
  * null
  * Number
  * Object
  * onBlur
  * onError
  * onFocus
  * onLoad
  * onUnload
  * open
  * opener
  * Option
  * outerHeight
  * outerWidth
  * **package**
  * Packages
  * pageXoffset
  * pageYoffset
  * **parent**
  * parseFloat
  * parseInt
  * Password
  * personalbar
  * Plugin
  * print
  * private
  * prompt
  * protected
  * prototype
  * public
  * Radio
  * ref
  * RegExp
  * releaseEvents
  * Reset
  * resizeBy
  * resizeTo
  * return
  * routeEvent
  * scroll
  * scrollbars
  * scrollBy
  * scrollTo
  * Select
  * self
  * setInterval
  * setTimeout
  * short
  * static
  * **status**
  * statusbar
  * stop
  * String
  * Submit
  * sun
  * super
  * switch
  * synchronized
  * taint
  * Text
  * Textarea
  * this
  * throw
  * throws
  * **toolbar**
  * **top**
  * toString
  * transient
  * true
  * try
  * typeof
  * unescape
  * untaint
  * unwatch
  * valueOf
  * var
  * void
  * watch
  * while
  * **window**
  * Window
  * with