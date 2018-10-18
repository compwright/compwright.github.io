---
id: 274
title: 'Uploading large files: covering all the bases'
date: 2011-05-05T06:27:45+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=274
permalink: /2011-05-05/uploading-large-files/
categories:
  - Notebook
tags:
  - Apache
  - PHP
  - uploading
---
When uploading a file to a PHP script on an Apache web server, there are several configuration options that if improperly set can get in the way. I just encountered yet _another_ one of these, and decided to catalog them here.

## Size, Time, and Memory

There are three types of limits that affect file uploads, and the weakest link in the chain is your effective limit.

If your size limit is set to 3gb, but your time limit does not allow for the time required to upload that much data, you&#8217;ll still be unable to upload those large files. Likewise, the ability to upload does no good if you do not have enough memory to process the file that was uploaded.

## Assumptions

This post assumes an 8MB upload limit (8mb x 1024kb x 1024 bytes = 8388608). You will want to adjust this number up or down according to your needs.

Oddly, although 8mb is the default value for PHP&#8217;s `upload_max_filesize` setting, some of the other default settings are much lower (2mb, or in some cases, only 100k).

## PHP limits

    upload_max_filesize = 8388608
    post_max_size = 8388608
    max_input_time = 60

Depending on what you&#8217;re doing with the uploaded files, you may also need to increase your memory limit:

    memory_limit = 64MB

## Apache limits

If <a href="http://httpd.apache.org/docs/2.0/mod/core.html#limitrequestbody" target="_blank"><code>LimitRequestBody</code></a> is set to something non-zero, you may need to increase its value in your Apache `httpd.conf` file or `.htaccess` file:

    LimitRequestBody 8388608

If you are using <a href="http://httpd.apache.org/mod_fcgid/" target="_blank"><code>mod_fcgid</code></a> (required to run the latest <a href="http://windows.php.net/download/#php-5.3-nts-VC9-x86" target="_blank">PHP 5.3 VC9 NTS build for Windows</a>), then you need to set the value of <a href="http://httpd.apache.org/mod_fcgid/mod/mod_fcgid.html#fcgidmaxrequestlen" target="_blank"><code>FcgidMaxRequestLen</code></a>, which defaults to 100k if it is not set. (Note that some systems may put `mod_fcgid` settings in a file separate from the main `httpd.conf` file).

    FcgidMaxRequestLen 8388608
    FcgidIOTimeout 60

Happy uploading!