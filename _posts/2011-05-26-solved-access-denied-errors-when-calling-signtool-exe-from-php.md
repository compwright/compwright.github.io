---
title: 'Solved: &#8220;Access Denied&#8221; errors when calling signtool.exe from
  PHP'
date: 2011-05-26 11:51:12 -04:00
permalink: "/2011-05-26/solved-access-denied-errors-when-calling-signtool-exe-from-php/"
categories:
- Notebook
tags:
- Apache
- code signing
- permissions
- PHP
- signtool.exe
- WAMP
id: 277
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=277
---

## SIGHntool, why must you give me such grief?

I have spent the last 8 hours trying to figure out why Microsoft&#8217;s `signtool.exe` code signing utility refuses to work when called from PHP&#8217;s system() or shell_exec() functions on my WAMP server:

<pre>C:\build&gt; "C:\Program Files\InstallMate 7\Tools\signtool.exe" sign /v /f codesignedcert.pfx Setup.exe 2&gt;&1

The following certificate was selected:
    Issued to: &lt;redacted&gt;.
    Issued by: UTN-USERFirst-Object
    Expires:   5/12/2012 6:59:59 PM
    SHA1 hash: &lt;redacted&gt;

Done Adding Additional Store

Attempting to sign: C:\build\Setup.exe

Number of files successfully Signed: 0
Number of warnings: 0
Number of errors: 1

SignTool Error: ISignedCode::Sign returned error: 0x80090010

	Access denied.

SignTool Error: An error occurred while attempting to sign: C:\build\Setup.exe</pre>

_Note: the `2>&1` at the end of the signtool call is essential if you want to capture error messages which are emitted to <a href="http://en.wikipedia.org/wiki/Standard_streams#Standard_error_.28stderr.29" target="_blank"><code>STDERR</code></a> instead of <a href="http://en.wikipedia.org/wiki/Standard_streams#Standard_output_.28stdout.29" target="_blank"><code>STDOUT</code></a>. Yes, I lost an hour or two just on that.
  
_ 

## Dead ends

  * Windows 7 apparently <a title="Fixing the Windows 7 Read Only Folder Blues" href="http://itexpertvoice.com/ad/fixing-the-windows-7-read-only-folder-blues/" target="_blank">sets the read-only attribute</a> on _all files_, and it isn&#8217;t easy to turn that attribute off. But since other file operations worked from PHP, this wasn&#8217;t the issue.
  * Prefacing the signtool call with `CMD /C` didn&#8217;t help.
  * Setting full control file permissions on the `C:\build` folder for Guest, SYSTEM, and any other user account I could think of didn&#8217;t help either.
  * Wrapping signtool in a batch file was an exercise in futility.

The maddeningly frustrating thing was that signtool worked great when called from the command line &#8212; just not from PHP!

## An aha! moment

The issue turned out to be pretty stupid, as they usually do. I merely had to change the account that Apache was running as to that of a normal user, instead of the default local system account.

[<img class="alignnone" src="/wp-content/uploads/2011/05/apache-service-user.png" alt="services.msc - changing the Apache user" width="100%" />](/wp-content/uploads/2011/05/apache-service-user.png)