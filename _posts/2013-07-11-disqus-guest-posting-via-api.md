---
title: Disqus guest posting via API
date: 2013-07-11 15:13:55 -04:00
permalink: "/2013-07-11/disqus-guest-posting-via-api/"
categories:
- Notebook
tags:
- API
- disqus
id: 416
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=416
---

While evaluating the <a href="http://disqus.com/" target="_blank">Disqus</a> <a href="http://disqus.com/api/docs/" target="_blank">API</a> for things like posting and flagging as a guest, I was baffled by this non-descript error:

    {"code":12,"response":"This application cannot create posts on the chosen forum"}

After checking the obvious things (like <a href="http://help.disqus.com/customer/portal/articles/832187-guest-commenting" target="_blank">enabling guest posting</a> and checking my domain settings for my forum and application), I was finally able to solve this using the [disqus-php](https://github.com/disqus/disqus-php) library:

    require __DIR__ . '/disqus-php-master/disqusapi/disqusapi.php';
    
    $disqus = new DisqusAPI($secret_key);
    
    print_r($disqus->posts->create(array(
        'thread' => $thread_id,
        'message' => $message,
        'author_name' => $author_name,
        'author_email' => $author_email,
        'api_key' => $api_key,
    )));
    

The catch is that the `api_key` is NOT the same thing as the public key shown in your Disqus application settings. I actually had to inspect one of the AJAX calls from the Disqus Javascript widget to get the correct `api_key`:

![Disqus AJAX call headers showing the api_key](http://i.stack.imgur.com/m2bif.png)