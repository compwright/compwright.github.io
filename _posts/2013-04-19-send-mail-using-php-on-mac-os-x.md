---
title: Send mail() using PHP on Mac OS X
date: 2013-04-19 10:21:43 -04:00
permalink: "/2013-04-19/send-mail-using-php-on-mac-os-x/"
categories:
- Notebook
tags:
- Mac
- mail
- PHP
- sendmail
id: 410
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=410
---

Thanks to <a href="http://benjaminrojas.net/configuring-postfix-to-send-mail-from-mac-os-x-mountain-lion/" target="_blank">Benjamin Rojas</a>, <a href="http://theandystratton.com/2009/fix-phps-mail-function-after-latest-os-x-leopard-update" target="_blank">Andy Stratton</a>, and a <a href="http://jspr.tndy.me/php-mail-and-osx-leopard/" target="_blank">tip from Jasper</a>, I was able to successfully send email from my home-brewed MAMP environment. Here&#8217;s the summary.

  1. Add the following to your `/etc/postfix/sasl_passwd` file: 
        smtp.gmail.com:587 username@gmail.com:password
    
    (Of course, you don&#8217;t have to use GMail or port 587, but you get the idea.)</li> 
    
      * Configure postfix: 
            sudo postmap /etc/postfix/sasl_passwd
    
      * Backup and edit your postfix configuration: 
            sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.orig
            sudo vim /etc/postfix/main.cf
        
        If you use TLS, then you will need to add the TLS settings but the other settings should already be there as a result of running the `postmap` command. You should have these options set in `/etc/postfix/main.cf`:
        
            mydomain_fallback = localhost
            mail_owner = _postfix
            setgid_group = _postdrop
            relayhost=smtp.gmail.com:587
            smtp_sasl_auth_enable=yes
            smtp_sasl_password_maps=hash:/etc/postfix/sasl_passwd
            smtp_sasl_security_options=
            smtp_use_tls=yes
            smtp_tls_security_level=encrypt
            tls_random_source=dev:/dev/urandom
    
      * Start postfix: 
            sudo postfix start
        
        If there are errors, you may need to edit your `/etc/postfix/main.cf` and restart postfix:
        
            sudo postfix reload
    
      * Send a test message: 
            date | mail -s test youremailaddress@yourdomain.com
    
      * Make postfix start automatically on boot by opening your `/System/Library/LaunchDaemons/org.postfix.master.plist` file and adding: 
            <key>RunAtLoad</key>
            <true/>
        
        Add this at the bottom just before the closing `</dict> tag.`</li> 
        
          * Edit your `/etc/php.ini` file and configure the `sendmail_path` option: 
                sendmail_path = "sendmail -t -i"</ol> 
        
        You should now be able to send email using PHP&#8217;s <a href="http://php.net/mail" target="_blank"><code>mail()</code></a> function. If you continue to have issues, watch the contents of your postfix mail log:
        
            tail -f /var/log/mail.log