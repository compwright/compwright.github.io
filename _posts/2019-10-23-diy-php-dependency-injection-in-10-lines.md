---
title: DIY PHP Dependency Injection in 10 lines
date: 2019-10-23 00:00:00 -0400
categories:
- Notebook
tags:
- DI
- PHP

---
Here's a use case for you: call a class method with a dynamic array of parameters, while ensuring the typing and presence of certain required parameters which may or may not be present in that array.

    class A {}
    class B {}
    
    class Foo {
    	public static function bar(B $b, A $a = null) {}
    }
    
    call_user_func_array(['Foo', 'bar'], [new B(), new A()]);

For this to work, the array elements must be in the correct order.

    // This won't work:
    $config = ['a' => new A(), 'b' => new B()];
    call_user_func_array(['Foo', 'bar'], $config);

Dynamically invoking a function is useful when working with [polymorphism](https://phpenthusiast.com/object-oriented-php-tutorials/polymorphism-in-php). But how do you do it when your array of arguments might have other data mixed in, or might be in a different order?

This is the very problem that dependency injection containers exist to solve. There are plenty of libraries (and whole frameworks) that do it well, but when the [YAGNI](https://www.martinfowler.com/bliki/Yagni.html) principle applies, you can do it in just 10 lines of code with a little help from PHP's built-in [Reflection module](https://www.php.net/reflection).

    function inject (array $container, $class, $method) {
    	extract($container);
    	$args = array_map(
    		function (ReflectionParameter $arg) {
    			return $arg->getName();
    		},
    		(new ReflectionMethod($class, $method))->getParameters()
    	);
    	return array_values(compact($args));
    }
    
    // This works:
    $config = ['a' => new A(), 'b' => new B()];
    $args = inject($config, 'Foo', 'bar');
    call_user_func_array(['Foo', 'bar'], $args);

Explanation:

* **Line 2:** extract the array elements out as variables in local scope
* **Lines 3-8:** inspect the method and get a simple array of parameter names that the method needs, in the correct order
* **Line 9:** compile an array of only those variables required to satisfy the method parameters, in the correct order, from the local scope variables created at line 2.

Side note: I really enjoy PHP's [compact](https://www.php.net/compact) function. I'm surprised I don't see more developers using it.