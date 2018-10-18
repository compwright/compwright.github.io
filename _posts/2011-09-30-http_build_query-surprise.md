---
id: 302
title: 'http_build_query(): surprise!'
date: 2011-09-30T08:04:52+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=302
permalink: /2011-09-30/http_build_query-surprise/
categories:
  - Notebook
tags:
  - API
  - http_build_query()
  - PHP
---
Did you know that PHP&#8217;s `http_build_query()` drops any key from your query array if the value is `NULL`? I wonder how many subtle API client bugs are caused by this behavior.

The workaround is easy, just set values that should be intentionally empty to boolean `FALSE`, integer 0, or an empty string.