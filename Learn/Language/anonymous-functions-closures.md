
* [knowledge Aggregation](#anonymous-functionsclosures)
* [Examples](#examples)
* [Resources](#resources)


# Anonymous functions/closures

Anonymous functions, also known as closures, allow the creation of functions which have **no specified name**. Because the function has no name, you can’t call it like a regular function. Instead you must either assign it to a variable ([Example](#example-3)) (PHP **automatically converts such expressions into instances of the Closure internal class**) or pass it to another function as an argument ([Example](#example-2)). 

Anonymous functions are implemented using the [Closure class](http://www.php.net//manual/en/class.closure.php).

Closures can also accept **regular arguments** ([Example](#example-1)).

A closure encapsulates its scope, meaning that **it has no access to the scope in which it is defined or executed**. 

 The parent scope of a closure is the function in which the closure **was declared (not necessarily the function it was called from)**. 

It is, however, possible **to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword** ([Example](#example-4)). Inherited variable's value is from when the function is defined, **not when called** ([Example](#example-6)). This inherits the variables **by-value**, that is, a copy is made available inside the closure using its original name. If you want you can inherit it by-reference ([Example](#example-7)), but remenber that if you inherit it from reference then if the parent scope changes its value, that change **will be reflected in the closure** ([Example](#example-8)). From PHP 7.1, these variables may not include superglobals, $this, or variables with the same name as a parameter.

You can use a closures in itself **via reference** ([Example](#example-5)).

You can **return a Closure from a function call** ([Example](#example-10)). 

You can **put a closure as a value of an associative array** and call it using array syntax followed by parenthesis ([Example](#example-16)).

Closures become useful when some piece of logic needs to be performed in a limited scope but retain the ability to interact with the environment external to that scope. They can be used as throw away bits of functionality that don’t pollute the global namespace and are good to use as part of a callback. they are useful for one offs or where it doesn’t make sense to define a function. They are also useful when using PHP functions that accept a callback function like array_map, array_filter, array_reduce or array_walk.

As of PHP7, you can **immediately execute anonymous functions** ([Example](#example-13)). 

Closures were introduced in PHP 5.3. As of PHP 5.4.0, when declared in the context of a class, **the current class is automatically bound to it, making $this available inside of the function's scope** ([Example](#example-9)). If this automatic binding of the current class is not wanted, then static anonymous functions may be used instead. As of PHP 5.4, anonymous functions may be declared statically ([Example](#example-11)). **This prevents them from having the current class automatically bound to them**. Objects may also **not be bound to them at runtime** ([Example](#example-14)).

If you try to call a closure **stored in an instance variable** as you would regularly do with methods, it will **give you an error** because php tries to match the instance method called with the name of the instance variable wich is not defined in the original class' signature ([Example](#example-15)).

Closures have additional object oriented uses as well. PHP 5.4 brings new methods to the Closure class’ interface. Specifically, the new bind and bindTo methods can be used to bind to new objects for the closure to operate on. Furthemore since the Closure class has the __invoke method, they can be type hinted as Callables ([Example](#example-17)).

 It is possible to use these functions func_num_args(), func_get_arg(), and func_get_args() from within a closure.
 
 You cannot access an array element inside the use() statement because it will give you an error  ([Example](#example-18)).
 
Anonymous functions can return references just like named functions can. Simply use the & the same way you would for a named function. right after the `function` keyword (and right before the nonexistent name) ([Example](#example-19)).

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
 
  
#### Example 9
#### closure declared in the context of a class, the current class is automatically bound to it, making $this available inside of the function's scope (only for PHP >= 5.4)
 
 
 ```php
  
 <?php

class Foo {
    
    private $color;

    function __construct($color) {
        $this->color = $color;
    }
    
     public function getProperty() {
         return function() {
           echo ucfirst($this->color); 
         };
     }
}// end of Class Foo

$a = new Foo('red');

$func = $a->getProperty();

$func(); //outputs 'Red'

 ```
 
#### Example 10 
#### closure returned from function call
 
 
 ```php
<?php

function say($message): callable {
   return function ($target) use ($message) {		
      echo "{$message} {$target}";
      };
}

$a = say('Hi');

$a('World'); //outputs 'Hi World';

 ```
 
 
#### Example 11 
#### static closure declaration
 
 
 ```php
<?php

$b = static function() { var_dump($this); };

$b() //get an notice and Null

$d = $b->bindTo(new StdClass());
$d(); //get a warning, a notice and null because you canot bind an static closure to an object

 ```

#### Example 12 
#### Attempting to use $this inside a static anonymous functionstatic closure declaration
 
 
 ```php
<?php

new class {
    function __construct()
    {
        (static function() {
            var_dump($this);
        })();
    }
};

//will output
//Notice: Undefined variable: this in %s on line %d
//NULL

 ```
 
#### Example 13 
#### Immediatily invoking an anonimous function
 
 
 ```php
<?php

(function() { echo 123; })(); // will print 123

 ```

#### Example 14 
#### Attempting to bind an object to a static anonymous function
 
 
 ```php
<?php

$b = static function() { var_dump($this); };

$d = $b->bindTo(new StdClass());

$d(); //get a warning, a notice and null because you canot bind an static closure to an object

 ```
 
 
#### Example 15
#### Trying to call a closure stored in a instance variable
 
 
 ```php
<?php

$obj = new StdClass();

$obj->func = function(){
echo "hello";
};

//$obj->func(); // doesn't work! php tries to match an instance method called "func" that is not defined in the original class' signature

// you have to do this instead:
$func = $obj->func;
$func();

// or:
call_user_func($obj->func);

 ```
 
#### Example 16
#### closure as a value of an associative array
 
 
 ```php
<?php

$array['func'] = function(){
echo "hello";
};

$array['func'](); // hello

 ```

#### Example 17
#### closure can be type hinted as Callables
 
 
 ```php
<?php

$a['crazy_func'] = function (): Callable {
    return function ($greeting) : Callable {
        return function($name) use ($greeting){
            return "{$greeting}  {$name} from Crazy func!";
        };
    };
};


echo $a['crazy_func']()('Hi')('Haiaty'); //outputs 'Hi Haiaty from Crazy func!'
echo $a['crazy_func']()('Hello')('Jhon'); //outputs 'Hello Jhon from Crazy func!'

 ```
 
#### Example 18
#### trying to acces array element inside use keyword
 
 
 ```php
<?php

$fruits = ['apples', 'oranges'];
$example = function () use ($fruits[0]) {
    echo $fruits[0]; 
};
$example(); //gives "Parse error: syntax error, unexpected '[', expecting ',' or ')' ... "

//Would have to do this:

$fruits = ['apples', 'oranges'];
$example = function () use ($fruits) {
    echo $fruits[0]; // will echo 'apples'
};
$example();


//Or this instead:


$fruits = ['apples', 'oranges'];
$fruit = $fruits[0];
$example = function () use ($fruit) {
    echo $fruit; // will echo 'apples'
};
$example();

 ```

#### Example 19
#### Returning a reference from an anonymous function
 
 
 ```php
<?php

$value = 0;

$fn = function &() use (&$value) { return $value; };

 $x =& $fn();
 
 var_dump($x, $value);        // 'int(0)', 'int(0)'
 
 ++$x;
    
 var_dump($x, $value);        // 'int(1)', 'int(1)'
    
 ```
 
 

# Resources
 
 * [Anonymous functions from PHP.net ](http://it2.php.net/manual/en/functions.anonymous.php)
 * [Closures from PHP.net ](http://www.php.net//manual/en/class.closure.php)
 * [Question about closures from an article on toptal.com](https://www.toptal.com/php)
 * [What are PHP Lambdas and Closures?] (http://culttt.com/2013/03/25/what-are-php-lambdas-and-closures/)
 * [Functional PHP] (https://leanpub.com/functional-php/read)
 

 


