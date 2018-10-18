---
id: 236
title: 'How to send e-mail&#8230;that gets delivered'
date: 2010-06-16T16:47:01+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=236
permalink: /2010-06-16/how-to-send-e-mail-that-gets-delivered/
categories:
  - Notebook
tags:
  - dkim
  - dns
  - E-Mail
  - email
  - Jeff Atwood
  - ptr
  - spam
---
I just got a really good education on how to make sure your (legit) email will navigate common spam blockers and be delivered successfully, <a title="So You'd Like to Send Some Email (Through Code)" href="http://www.codinghorror.com/blog/2010/04/so-youd-like-to-send-some-email-through-code.html" target="_blank">thanks to Jeff Atwood</a>.

## Summary

  1. **Make sure the computer sending the email has a <a href="http://aplawrence.com/Blog/B961.html" target="_blank">Reverse PTR record</a>**. Your ISP has to do it, not your DNS provider or web host.
  2. **Sign your messages using <a href="http://en.wikipedia.org/wiki/DKIM" target="_blank">DomainKeys Identified Mail</a>**. Requires DNS and code changes.
  3. **Set up a <a href="http://en.wikipedia.org/wiki/Sender_ID" target="_blank">SenderID DNS record</a>.** Far less critical than the first two, but still nice to have.

## Did it work?

  1. **Send a message to a GMail account**&#8211;they provide excellent diagnostic headers. Look for `Received-SPF` and `Authentication-Results`.
  2. **Use the [Port25 diagnostic service](mailto:check-auth@verifier.port25.com)** (check-auth@verifier.port25.com). You can ignore a DomainKeys check fail if the DKIM check passes.