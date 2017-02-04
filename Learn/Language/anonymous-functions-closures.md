
* [knowledge Aggregation](#anonymous-functionsclosures)
* [Examples](#examples)
* [Resources](#resources)


# Anonymous functions/closures

Anonymous functions, also known as closures, allow the creation of functions which have no specified name. They are most useful as the value of callback parameters ([Example](#example-2)), but they have many other uses.

Anonymous functions are implemented using the [Closure class](http://www.php.net//manual/en/class.closure.php).

Closures can also be used as the values of variables; PHP automatically converts such expressions into instances of the Closure internal class.  ([Example](#example-3))

Closures can also accept regular arguments ([Example](#example-1)).

A closure encapsulates its scope, meaning that it has no access to the scope in which it is defined or executed. 

 The parent scope of a closure is the function in which the closure was declared (not necessarily the function it was called from). 

It is, however, possible to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword ([Example](#example-4)). Inherited variable's value is from when the function is defined, not when called ([Example](#example-6)). This inherits the variables by-value, that is, a copy is made available inside the closure using its original name. If you want you can inherit it by-reference ([Example](#example-7)), but remenber that if you inherit it from reference then if the parent scope changes its value, that change will be reflected in the closure ([Example](#example-8)). From PHP 7.1, these variables may not include superglobals, $this, or variables with the same name as a parameter.

You can use a closures in itself via reference ([Example](#example-5)).

Closures become useful when some piece of logic needs to be performed in a limited scope but retain the ability to interact with the environment external to that scope. 

As of PHP 5.4.0, when declared in the context of a class, the current class is automatically bound to it, making $this available inside of the function's scope. If this automatic binding of the current class is not wanted, then static anonymous functions may be used instead.

Closures have additional object oriented uses as well. PHP 5.4 brings new methods to the Closure class’ interface. Specifically, the new bind and bindTo methods can be used to bind to new objects for the closure to operate on

---

# Examples


#### Example 1
#### Closures accepting regular parameters


 ```php
 
 <?php
 
$example = function ($arg) {
   echo $arg;
};
$example("hello");

//prints 'hello'
 
 ```


#### Example 2
#### Anonymous function as a callback

 ```php
 
 <?php
 
echo preg_replace_callback('~-([a-z])~', function ($match) {
    return strtoupper($match[1]);
}, 'hello-world');

// outputs helloWorld
?>
 
 
 ```
 
#### Example 3
#### Anonymous function as value of variables

 ```php
 
<?php

$greet = function($name)
{
    printf("Hello %s\r\n", $name);
};


$greet('World');

$greet('PHP');


?>
 
 ```

#### Example 4
#### inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword


 ```php
 <?php
 
 $var = 'World';
 
 
 $closure = function () use ($var) {
 
 echo "Hello {$var}" ;
 
 };
 
 $closure(); // "Hello World"
 
 ```

#### Example 5
#### Using closure in itself via reference
 
 
 ```php
  
 <?php
 
$deleteDirectory = null;

$deleteDirectory = function($path) use (&$deleteDirectory) {
    $resource = opendir($path);
    while (($item = readdir($resource)) !== false) {
        if ($item !== "." && $item !== "..") {
            if (is_dir($path . "/" . $item)) {
                $deleteDirectory($path . "/" . $item); //referencing the closure
            } else {
                unlink($path . "/" . $item);
            }
        }
    }
    closedir($resource);
    rmdir($path);
};

$deleteDirectory("path/to/directoy");
 
 
 ```

#### Example 6
#### Inherited variable's value is from when the function is defined, not when called 
 
 
 ```php
  
 <?php
       
$message = 'hello';

$example = function () use ($message) {
    var_dump($message);
};

$message = 'world';
$example();
 
 //prints 'hello'
 
 ```
 
#### Example 7
#### Inheriting variable from parent scope by reference
 
 
 ```php
  
 <?php
 
$message = 'hello';

$example = function () use (&$message) {
    var_dump($message);
};
$example();
 
 //prints 'hello'
 
 ```
 
#### Example 8
#### Inheriting variable from parent scope by reference and changes on parent scope is reflecte on closure
 
 
 ```php
  
 <?php
 
$message = 'hello';

$example = function () use (&$message) {
    var_dump($message);
};
$example();  //prints 'hello'

$message = 'changed from parent Scope!';

$example();  //prints 'changed from parent Scope!'

 ```
 

# Resources
 
 * [Anonymous functions from PHP.net ](http://it2.php.net/manual/en/functions.anonymous.php)
 * [Closures from PHP.net ](http://www.php.net//manual/en/class.closure.php)
 * [Question about closures from an article on toptal.com](https://www.toptal.com/php)
 

 

