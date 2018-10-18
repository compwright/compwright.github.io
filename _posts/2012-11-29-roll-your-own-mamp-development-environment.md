---
id: 370
title: Roll your own MAMP development environment
date: 2012-11-29T09:54:24+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=370
permalink: /2012-11-29/roll-your-own-mamp-development-environment/
categories:
  - Notebook
tags:
  - Apache
  - MAMP
  - MySQL
  - PHP
---
Pre-packaged MAMP, LAMP, and WAMP stacks have been common on developer&#8217;s computers for years. Such packages are convenient because they provide a single-step install process, with all components in the server stack preconfigured to work together, and off you go.

[Except when they don&#8217;t](/2012-06-22/cannot-run-a-binary-executable-from-php-and-mamp/ "Cannot run a binary executable from PHP and MAMP?").

I&#8217;ve learned from experience that these packages have ways of making you pay for the convenience you enjoyed up front. If you have ever needed to:

  * Install a PHP extension that wasn&#8217;t already provided in your stack
  * Run a specific version of PHP or MySQL
  * Install PEAR packages
  * Install SSL certificates
  * Run command-line PHP scripts

&#8230;you may have encountered some ugly, time-wasting surprises along the way.

It pays to know your environment inside and out. Today, it is quite easy to roll your own Apache-MySQL-PHP stack on <a title="Windows Apache PHP FastCGI Setup" href="http://www.farinspace.com/windows-apache-php-fastcgi/" target="_blank">Windows</a>, <a title="Installing Apache, MySQL, and PHP on Ubuntu Linux" href="https://help.ubuntu.com/community/ApacheMySQLPHP" target="_blank">Linux</a>, or even Mac OS X.<!--more-->

## Getting started

This tutorial will show you how to roll your own MAMP stack with Mac OS X 10.7+ (Lion), Apache 2, MySQL 5.5, and PHP 5.4. If you wish to use different versions, a similar procedure should work but the specifics will probably vary.

For reference, I have provided a reference script which was the basis for this blog post: <a href="https://gist.github.com/4170773" target="_blank"><strong>build-mamp-osx.sh</strong></a>.

We will perform the following steps:

  1. Install dependencies needed for PHP
  2. <span style="line-height: 1.714285714; font-size: 1rem;">Compile PHP from scratch</span>
  3. Install MySQL
  4. Configure PHP, MySQL, and Apache to run virtual hosts

<div>
  <em><span style="line-height: 24px;">Note: OS X Lion and higher already have Apache installed.</span></em>
</div>

## 1. Install PHP dependencies

  1. Verify that you have the latest <a href="https://developer.apple.com/xcode/" target="_blank">Apple XCode</a> from the App Store. If you do not already have XCode, you may need to upgrade to the latest OS X.
  2. Download and install XQuartz from <a href="http://xquartz.macosforge.org/" target="_blank">http://xquartz.macosforge.org/</a>. <span style="line-height: 1.714285714; font-size: 1rem;">After installing XQuartz, run the following:</span> 
    <pre>sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
ln -s /opt/X11 /usr/X11</pre>

  3. <a href="http://brew.sh" target="_blank">Install Homebrew</a> and run `brew doctor` to verify that it is working correctly.
  4. Using Homebrew, install the following libraries: 
    <pre>brew install libpng
brew install libjpeg
brew install gd
brew install pcre
brew install libxml2
brew install mcrypt
brew install icu4c
brew install wget</pre>

  5. Install the IMAP-2007f library: 
    <pre>wget ftp://ftp.cac.washington.edu/imap/imap-2007f.tar.gz
tar zxvf imap-2007f.tar.gz
cd imap-2007f
make osx EXTRACFLAGS="-arch i386 -arch x86_64 -g -Os -pipe -no-cpp-precomp"
sudo cp c-client/*.h /usr/local/include/
sudo cp c-client/*.c /usr/local/lib
sudo cp c-client/c-client.a /usr/local/lib/libc-client.a
cd ..</pre>

## 2. Compile PHP

  1. Download and extract the latest PHP 5.4 source tarball from <a href="http://php.net/downloads.php" target="_blank">http://php.net/downloads.php</a>: 
    <pre>wget http://us3.php.net/get/php-5.4.9.tar.gz/from/this/mirror
tar xzvf php-5.4.9.tar.gz
cd php-5.4.9</pre>

  2. Configure the makefile: 
    <pre>./configure \
--prefix=/usr \
--mandir=/usr/share/man \
--infodir=/usr/share/info \
--sysconfdir=/private/etc \
--with-apxs2=/usr/sbin/apxs \
--enable-cli \
--with-config-file-path=/etc \
--with-libxml-dir=/usr \
--with-openssl=/usr \
--with-kerberos=/usr \
--with-zlib=/usr \
--enable-bcmath \
--with-bz2=/usr \
--enable-calendar \
--with-curl=/usr \
--enable-dba \
--enable-exif \
--enable-ftp \
--with-gd \
--enable-gd-native-ttf \
--with-icu-dir=/usr/local \
--with-iodbc=/usr \
--with-ldap=/usr \
--with-ldap-sasl=/usr \
--with-libedit=/usr \
--enable-mbstring \
--enable-mbregex \
--with-mysql=mysqlnd \
--with-mysqli=mysqlnd \
--without-pear \
--with-pdo-mysql=mysqlnd \
--with-mysql-sock=/var/mysql/mysql.sock \
--with-readline=/usr \
--enable-shmop \
--with-snmp=/usr \
--enable-soap \
--enable-pcntl \
--enable-sockets \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--with-tidy \
--enable-wddx \
--with-xmlrpc \
--with-iconv-dir=/usr \
--with-xsl=/usr \
--enable-zip \
--with-imap=/usr/local/imap-2007 \
--with-kerberos \
--with-imap-ssl \
--enable-intl \
--with-pcre-regex \
--with-pgsql=/usr \
--with-pdo-pgsql=/usr \
--with-freetype-dir=/usr/X11 \
--with-jpeg-dir=/usr \
--with-png-dir=/usr/X11</pre>
    
    Verify that this completed without errors before proceeding. If you have installed the dependencies listed in the previous step, there should be none.</li> 
    
      * Compile and install the PHP binary: 
        <pre>make
sudo make install</pre>
    
      * Enable `mod_php` and virtual hosts in your Apache configuration by adding the following line to your `/etc/apache2/httpd.conf` file: 
        <pre>LoadModule php5_module /usr/local/Cellar/php54/5.4.9/libexec/apache2/libphp5.so</pre>
    
      * Restart apache:
      * <pre>sudo apachectl restart</pre></ol> 
    
    ## 3. Install MySQL
    
      1. Download and install the latest <a href="http://www.mysql.com/downloads/mysql/" target="_blank">MySQL 5.5 Community Server</a> binary (DMG package). Within the DMG archive, be sure to install all three components: the MySQL package, the MySQL.prefPane, and the MySQLStartupItem package.
      2. Create your MySQL configuration file: 
        <pre>sudo cp /usr/local/mysql/support-files/my-small.cnf /etc/my.cnf</pre>
    
      3. Edit your `./profile` file and add the `/usr/local/mysql/bin` directory to the path. You will need to restart Terminal for this to take effect. 
        <pre>export PATH=/usr/local/mysql/bin:/usr/local/opt/php54/bin:$PATH</pre>
    
      4. Restart MySQL using the System Preferences MySQL pane.
      5. Verify that MySQL is running on localhost: 
        <pre>mysql -u root</pre>
        
        It is recommended that you <a href="http://www.cyberciti.biz/faq/mysql-change-root-password/" target="_blank">set a root password</a> at this point.</li> </ol> 
        
        ## Adding Virtual Hosts
        
        Once your MAMP stack in set up, here is how to create virtual hosts:
        
          1. Add your virtual host domain name to the `/etc/hosts` file (requires Administrator or `sudo` access): 
            <pre>127.0.0.1 helloworld.dev</pre>
        
          2. Add a virtual host block to your `/private/etc/apache2/extra/httpd-vhosts.conf` file (requires Administrator or `sudo` access): 
            <pre>&lt;VirtualHost *:80&gt;
	ServerName helloworld.dev
	DocumentRoot "/Users/jhill/Sites/helloworld.dev"
	&lt;Directory "/Users/jhill/Sites/helloworld.dev"&gt;
		Options Includes FollowSymLinks  
		AllowOverride All
		Order allow,deny
 		Allow from all
	&lt;/Directory&gt;
&lt;/VirtualHost&gt;</pre>
        
          3. Restart Apache: 
            <pre>sudo apachectl restart</pre>
        
          4. Add an `index.php` file in your virtual host document root, `~/Sites/helloworld.dev/`: 
            <pre>&lt;?php echo 'Hello, world!';</pre>
        
          5. Browse to **http://helloworld.dev/** in your web browser.
        
        ## Conclusion
        
        Building your own MAMP stack with Mac OS X 10.7+, MySQL 5.5, and PHP 5.4 is easier than you think, and offers the advantages of total flexibility, less &#8220;magic&#8221;, better reliability, and a greater understanding of where things are and how to fix them if they break.
        
        Did you try it? How did it go?