---
title: How to handle headers and footers in CodeIgniter
date: 2009-08-27 14:14:24 -04:00
permalink: "/2009-08-27/how-to-handle-headers-and-footers-in-codeigniter/"
categories:
- Notebook
tags:
- CodeIgniter
- Footers
- Headers
- PHP
- Views
id: 185
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=185
---

I got this question from a reader and thought it would be useful to post for everyone:

> Hi Jonathon,
> 
> I really like some of your solutions to making things simpler when using CodeIgniter. On the subject, I was wondering if you had a preference for a simple way to include headers and footers in your views. I know you can either do views within views, but that just doesn&#8217;t feel right to me. Although the advantage is only having to use one line to call your views within a controller. The method of building your views within an action by calling each view seperately also doesn&#8217;t interest me, because I&#8217;m trying to adhere to the DRY principle. I&#8217;ve also seen a solution where you create your own MY_controller and build the view that way then inherit from all your other controllers.
> 
> Do you have a recommended way to include your common views that&#8217;s simple and doesn&#8217;t require you add multiple lines of code to each controller action? I&#8217;m trying to keep my view calling to one line within each action of the controller.
> 
> Thanks,
> 
> Lee

My preference on that is simply calling views within views. It just makes sense to do it that way. If there is data that must be passed to the header and footer globally, then I would extend the Loader class with a global data function like this:

    class MY_Loader extends CI_Loader {
    
    	function MY_Loader() {
    		parent::CI_Loader();
    	}
    
    	function global_view_data($key, $val) {
    		# normalize the data array
    		$data = (is_array($key))? $key : array($key => $val);
    
    		# merge into the view data array
    		$this->_ci_cached_vars = array_merge($this->_ci_cached_vars, $data);
    	}
    
    }

Copy that to a file called **MY_Loader.php** and save it in your libraries folder <a rel="nofollow" href="http://codeigniter.com/user_guide/general/core_classes.html">as described in the user guide</a>. Once that is done you can load data which will be passed to all views globally:

    $this->load->global_view_data($stuff);

Note: I have found that including views within views is aggravating without a helper function, so I wrote the <a rel="nofollow" href="http://jonathonhill.net/codeigniter/modularity/">Modularity plugin</a> to help with that.