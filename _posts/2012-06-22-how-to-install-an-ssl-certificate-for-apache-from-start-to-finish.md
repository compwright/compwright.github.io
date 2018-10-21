---
title: How to install an SSL certificate for Apache, from start to finish
date: 2012-06-22 09:45:09 -04:00
permalink: "/2012-06-22/how-to-install-an-ssl-certificate-for-apache-from-start-to-finish/"
categories:
- Notebook
tags:
- Apache
- SSL
id: 338
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=338
---

  1. ## Create an SSL key to use to generate the certificate signing request
    
    (Save this, you&#8217;ll need it to install the certificate). To generate the keys for the Certificate Signing Request (CSR) run the following command from a terminal prompt:
    
    <pre>openssl genrsa -des3 -out server.key 1024
Generating RSA private key, 1024 bit long modulus
.....................++++++
.................++++++
unable to write 'random state'
e is 65537 (0x10001)
Enter pass phrase for server.key:</pre>
    
    Enter a passphrase.
    
    Now we&#8217;ll remove the passphrase from the key, so that you don&#8217;t have to enter this passphrase whenever you restart Apache:
    
    <pre>openssl rsa -in server.key -out server.key.insecure
mv server.key server.key.secure
mv server.key.insecure server.key</pre>

  2. ## Generate a certificate signing request
    
    <pre>openssl req -new -key server.key -out server.csr</pre>
    
    It will prompt you to enter Company Name, Site Name, Email Id, etc. Once you enter all these details, your CSR will be created and it will be stored in the server.csr file.
    
    You can now submit this CSR file to a Certificate Authority (CA) for processing. The CA will use this CSR file and issue the certificate.</li> 
    
      * ## Purchase an SSL certificate
        
        You will be asked to supply the CSR that you generated in #2.</li> 
        
          * ## Install the SSL key from #1, the SSL certificate from #3, and the SSL issuer root certificates (aka &#8220;bundle&#8221; or &#8220;chain&#8221;).
            
            On an Ubuntu server, I usually upload the files here:
            
            <pre>/etc/apache2/ssl/domain.com.key
/etc/apache2/ssl/domain.com.crt
/etc/apache2/ssl/domain.com.bundle</pre>
        
          * ## Modify your Apache vhost
            
            Note: Apache only supports one SSL vhost per IP address.
            
            Replace {ip_address} with the public IP address of the server:
            
            <pre>&lt;VirtualHost {ip_address}:443&gt;
    DocumentRoot /var/www/vhosts/domain.com

    SSLEngine on
    SSLVerifyClient none
    SSLCertificateFile /etc/apache2/ssl/domain.com.crt
    SSLCertificateKeyFile /etc/apache2/ssl/domain.com.key
    SSLCertificateChainFile /etc/apache2/ssl/domain.com.bundle

    &lt;Directory /var/www/vhosts/domain.com&gt;
        AllowOverride All
        order allow,deny
        allow from all
        Options -Includes -ExecCGI
        AddOutputFilterByType DEFLATE text/html text/plain text/css text/xml application/x-javascript
    &lt;/Directory&gt;
&lt;/VirtualHost&gt;</pre>
        
          * ## Restart Apache
            
            <pre>/etc/init.d/apache2 restart</pre></ol> 
        
        That&#8217;s all!