---
id: 181
title: Easy startup scripts using crontab
date: 2009-08-18T06:53:59+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=181
permalink: /2009-08-18/easy-startup-scripts-using-crontab/
categories:
  - Notebook
tags:
  - Bootup
  - crontab
  - Linux
  - Reboot
  - Scripts
  - Startup
---
There is a super-easy way to start a program during system boot. Just put this in your crontab:

    @reboot /path/to/my/program

The command will be executed on every (re)boot. Crontab can be modified by running:

    sudo crontab -e