---
title: Performing a bitwise NOT on arbitrarily long integers
date: 2009-09-04 09:34:00 -04:00
permalink: "/2009-09-04/performing-a-bitwise-not-on-arbitrarily-long-integers/"
categories:
- Notebook
tags:
- 64-bit
- Boolean Logic
- GMP
- PHP
id: 191
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=191
---

Here&#8217;s the surprisingly simple solution to a fairly challenging problem. I do not understand why PHPs GMP extension does not include a gmp_not() function.

    function gmp_not($n) {
    	
    	# convert to binary string
    	$n = gmp_strval($n, 2);
    	
    	# invert each bit, one at a time
    	for($i = 0; $i < strlen($n); $i++) {
    		$n[$i] = ~$n[$i];
    	}
    	
    	# convert back to decimal
    	return gmp_strval(gmp_init($n, 2), 10);
    }