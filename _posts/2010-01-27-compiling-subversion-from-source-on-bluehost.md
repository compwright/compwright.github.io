---
id: 223
title: Compiling subversion from source on Bluehost
date: 2010-01-27T07:13:29+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=223
permalink: /2010-01-27/compiling-subversion-from-source-on-bluehost/
categories:
  - Notebook
tags:
  - Bluehost
  - Shared Hosting
  - Subversion
---
I recently had to [install the subversion client](http://www.bluehostforum.com/showpost.php?p=51455&postcount=19) in a shared hosting environment (specifically [Bluehost](http://bluehost.com), but these instructions probably work with other web hosts as well). It goes like this:

**1) Add these lines into `~/.bash_profile`**

<pre class="bash">export PYTHONPATH="$HOME/lib/python2.3/site-packages"
export LD_LIBRARY_PATH="$HOME/lib"</pre>

**2) Download the subversion source code**

<pre class="bash">mkdir ~/src
cd ~/src
wget http://subversion.tigris.org/downloads/subversion-1.4.6.tar.gz
wget http://subversion.tigris.org/downloads/subversion-deps-1.4.6.tar.gz
tar -xzvf subversion-1.4.6.tar.gz
tar -xzvf subversion-deps-1.4.6.tar.gz
cd subversion-1.4.6</pre>

**3) Compile dependencies**

<pre class="bash">cd apr
./configure --enable-shared --prefix=$HOME
make && make install

cd ../apr-util
./configure --enable-shared --prefix=$HOME --with-expat=builtin --with-apr=$HOME --without-berlekey-db
make && make install

cd ../neon
EXTRA_CFLAGS="-L/usr/lib64 -fPIC"
CFLAGS="-L/usr/lib64 -fPIC"
./configure --prefix=/home/zzzzz/system --enable-shared --with-ssl
make && make install</pre>

_Note: replace `zzzzz` with your user directory._

**4) Compile subversion**

<pre class="bash">cd ..
./configure --prefix=/home/zzzzz/system --with-expat=builtin --with-ssl --with-neon=/usr/lib64
make && make install</pre>

_Note: replace `zzzzz` with your user directory._

**5) Edit `~/.bash_profile` to add `~/system/bin` to your path**

<pre class="bash">vim ~/.bash_profile</pre>

**replace:**

<pre class="bash">PATH=$PATH:$HOME/bin</pre>

**with:**

<pre class="bash">PATH=$PATH:$HOME/bin:$HOME/system/bin</pre>

**6) Logout of your session and then log back in again. Subversion should now be working.**