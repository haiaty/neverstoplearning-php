
* [knowledge Aggregation](#anonymous-functionsclosures)
* [Examples](#examples)
  * [Anonymous function as value of variables](#anonymous-function-as-value-of-variables)
  * [Anonymous function as a callback] (#anonymous-function-as-a-callback)
  * [Closures accepting regular parameters](#closures-accepting-regular-parameters)
  * [inherit variables from the parent scope into the closure with the use keyword](#inherit-variables-from-the-parent-scope-into-the-closure-with-the-use-keyword)
  * [Closure returned from function call](#closure-returned-from-function-call)
  * [Inheriting variable from parent scope by reference](#inheriting-variable-from-parent-scope-by-reference)
  * [Using closure in itself via reference](#using-closure-in-itself-via-reference)
  * [Changes on variables inherited by reference are reflected inside the closure](#changes-on-variables-inherited-by-reference-are-reflected-inside-the-closure)
  * [Closure declared in the context of a class](#closure-declared-in-the-context-of-a-class)
  * [Static closure declaration](#static-closure-declaration)
  * [Attempting to bind an object to a static anonymous function](#attempting-to-bind-an-object-to-a-static-anonymous-function)
  * [Type hinting closures as Callables](#type-hinting-closures-as-callables)
  * [Returning a reference from an anonymous function](#returning-a-reference-from-an-anonymous-function)
  * [Accessing private method of a class inside a Closure](#accessing-private-method-of-a-class-inside-a-Closure)
  * [Closure::bindTo() example](#closurebindto-example)
  * [Closure::bind() example](#closurebind-example)
  * [Closure call method usage example](#closure-call-method-usage-example)
* [Tricks, tips and crazy things](#tricks-tips-and-crazy-things)
  * [Closure as a value of an associative array](#closure-as-a-value-of-an-associative-array)
  * [Immediatily invoking an anonimous function](#immediatily-invoking-an-anonimous-function)
  * [can't access array element inside use keyword](#cant-access-array-element-inside-use-keyword)
  * [can't use $this in use keyword](#cant-use-$this-in-use-keyword)
  * [Inherited variable's value is from when the function is defined, not when called](#inherited-variables-value-is-from-when-the-function-is-defined-not-when-called)
  * [using anonymous function to change a method at runtime](#using-anonymous-function-to-change-a-method-at-runtime)
  * [How to call a closure stored in a instance variable](#how-to-call-a-closure-stored-in-a-instance-variable)
  * [$this variable is undefined inside static function](#this-variable-is-undefined-inside-static-function)
  * [Adding public functions to class using closures](#adding-public-functions-to-class-using-closures)
  * [Calling closures assigned to class properties as class methods](#calling-closures-assigned-to-class-properties-as-class-methods)
  * [check whether you're dealing with a closure specifically](#check-whether-youre-dealing-with-a-closure-specifically)
  * [Adding method on the fly to objects using the Closure::bind method](#adding-method-on-the-fly-to-objects-using-the-closurebind-method)
  * [validate whether or not a closure can be bound to a PHP object](#validate-whether-or-not-a-closure-can-be-bound-to-a-php-object)
  * [using Closure BindTo method to create a small template engine](#using-closure-bindto-method-to-create-a-small-template-engine)
* [Resources](#resources)


# Anonymous functions/closures

Anonymous functions (someone call them Lambda functions), also known as closures (in PHP), allow the creation of functions which have **no specified name**.They were introduced in PHP 5.3.Because the function has no name, you can’t call it like a regular function. Instead you must either **assign it to a variable** ([Example](#anonymous-function-as-value-of-variables)) (PHP automatically converts such expressions into instances of the **Closure internal class**) or **pass it to another function as an argument (Callback)** ([Example](#anonymous-function-as-a-callback)).

The difference between an anonymous function and a closure is basically that the closure are anonymous functions that have acces to the parent scope using the use keyword. This is why PHP treat anonymous functions as Closure objects. In fact anonymous functions are implemented using the [Closure class](http://www.php.net//manual/en/class.closure.php). So if you want you can type hint it as Closure type (If you are using namespaces, make sure you give a fully qualified namespace.).

Closures become useful when some piece of logic needs to be performed in a limited scope but retain the ability **to interact with the environment external to that scope**. They can be used as throw away bits of functionality that don’t pollute the global namespace and are good to use as part of a callback. they are useful for one offs or where it doesn’t make sense to define a function. They are also useful when using PHP functions that accept a callback function like array_map, array_filter, array_reduce or array_walk.

The syntax for closures is:

```php

function & (parameters) use (lexical vars) { body }

//The & is optional and indicates that the function should return a reference.
//The use followed by the parentheses is optional and
//indicates that several variables from the current scope should be imported into the closure.

```

Closures can accept **regular arguments** ([Example](#closures-accepting-regular-parameters)).

A closure encapsulates its scope, meaning that **it has no access to the scope in which it is defined or executed**.

 The parent scope of a closure is the function in which the closure **was declared (not necessarily the function it was called from)**.

It is, however, possible **to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword** ([Example](#inherit-variables-from-the-parent-scope-into-the-closure with the use keyword)). Inherited variable's value is from when the function is defined, **not when called** ([Example](#inherited-variables-value-is-from-when-the-function-is-defined-not-when-called)). This inherits the variables **by-value**, that is, a copy is made available inside the closure using its original name. If you want you can inherit it by-reference ([Example](#inheriting-variable-from-parent-scope-by-reference)), but remenber that if you inherit it from reference then if the parent scope changes its value, that change **will be reflected in the closure** ([Example](#changes-on-variables-inherited-by-reference-are-reflected-inside-the-closure)). From PHP 7.1, these variables may not include superglobals, $this, or variables with the same name as a parameter.

You can use a closures in itself **via reference** ([Example](#using-closure-in-itself-via-reference)).

You can **return a Closure from a function call** ([Example](#closure-returned-from-function-call)).

You can **put a closure as a value of an associative array** and call it using array syntax followed by parenthesis. You cannot do the same with the array assignment operator. ([Example](#closure-as-a-value-of-an-associative-array))

As of PHP7, you can **immediately execute anonymous functions** ([Example](#immediatily-invoking-an-anonimous-function)).

As of PHP 5.4.0 when declared in the context of a class, **the current class is automatically bound to it, making $this available inside of the function's scope** ([Example](#closure-declared-in-the-context-of-a-class)). If this automatic binding of the current class is not wanted, then static anonymous functions may be used instead. As of PHP 5.4, anonymous functions may be declared statically ([Example](#static-closure-declaration)). **This prevents them from having the current class automatically bound to them**. Objects may also **not be bound to them at runtime** ([Example](#attempting-to-bind-an-object-to-a-static-anonymous-function)). PHP 5.4 now allows accessing private and protected members of an object if it's passed into an anymous function ([Example](#accessing-private-method-of-a-class-inside-a-Closure))

If you try to call a closure **stored in an instance variable** as you would regularly do with methods, it will **give you an error** because php tries to match the instance method called with the name of the instance variable wich is not defined in the original class' signature ([Example](#how-to-call-a-closure-stored-in-a-instance-variable)).

Closures have additional object oriented uses as well. PHP 5.4 brings new methods to the Closure class’ interface. Specifically, the new bind and bindTo methods can be used to bind to new objects for the closure to operate on. Furthemore since the Closure class has the __invoke method, they can be **type hinted as Callables** ([Example](#type-hinting-closures-as-callables)).

The BindTo method of the Closure class Create and return a new anonymous function with the same body and bound variables as this one, but possibly with a different bound object and a new class scope. The “bound object” determines the value $this will have in the function body and the “class scope” represents a class which determines which private and protected members the anonymous function will be able to access. Namely, the members that will be visible are the same as if the anonymous function were a method of the class given as value of the newscope parameter ([Example](#closurebindto-example)). Static closures cannot have any bound object (the value of the parameter newthis should be NULL), but this function can nevertheless be used to change their class scope. Note: If you only want to duplicate the anonymous functions, you can use cloning instead.

The Bind method of the Closure class duplicates a closure with a specific bound object and class scope.([Example](#closurebind-example)) 

The Call method binds the closure to an speific objct, calls the closure and returns the return of the closure. ([Example]((#closure-call-method-usage-example)))
 
 It is possible to use these functions func_num_args(), func_get_arg(), and func_get_args() from within a closure.

 You **cannot access an array element inside the use** keyword because it will give you an error  ([Example](#cant-access-array-element-inside-use-keyword)). Also you can't use the $this variable inside the use keyword ([Example](#cant-use-$this-in-use-keyword)).

Anonymous functions **can return references** just like named functions can. Simply use the & the same way you would for a named function. right after the `function` keyword (and right before the nonexistent name) ([Example](#returning-a-reference-from-an-anonymous-function)).

You can't **serialize a  PHP Closure object**. If you try  you get a very specific error message from the PHP Runtime: 'Uncaught exception 'Exception' with message 'Serialization of 'Closure' is not allowed''. If you need to serialize a closure try to use this library [PHP SuperClosure](https://github.com/jeremeamia/super_closure)

After you have assigned the cloure to a variable, let us suppose a variable called $lambda,  you can call it or using the name of the variable and () (in this case $lambda())or you can use the functions call_user_func($lambda) and call_user_func_array ($lambda, array ());

Since closures are anonymous, they do not appear in reflection.

However, a new method was added to the ReflectionMethod and ReflectionFunction classes: getClosure. This method returns a dynamically created closure for the specified function. ([Example](#reflection-class-method-getClosure))

#### Common misconceptions

Lambda functions / closures are not a way of dynamically extending classes by additional methods at runtime. There are several other possibilities to do this, including __call semantic.

PHP's notion of scope is quite different than the notion of scope other languages define. Combine this with variable variables ($$var) and it becomes clear that automatically detecting which variables from the outer scope are referenced inside are closure is impossible. Also, since for example global variables are not visible inside functions either by default, automatically making the parent scope available would break with the current language concept PHP follows

---

# Examples

#### Closures accepting regular parameters


 ```php

 <?php

$example = function ($arg) {
   echo $arg;
};
$example("hello");

//prints 'hello'

 ```

#### Anonymous function as a callback

 ```php

 <?php

echo preg_replace_callback('~-([a-z])~', function ($match) {
    return strtoupper($match[1]);
}, 'hello-world');

// outputs helloWorld
?>


 ```

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

#### inherit variables from the parent scope into the closure with the use keyword


 ```php
 <?php

 $var = 'World';


 $closure = function () use ($var) {

 echo "Hello {$var}" ;

 };

 $closure(); // "Hello World"

 ```

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

#### Changes on variables inherited by reference are reflected inside the closure


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


#### closure declared in the context of a class


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


#### static closure declaration


 ```php
<?php

$b = static function() { var_dump($this); };

$b() //get an notice and Null

$d = $b->bindTo(new StdClass());
$d(); //get a warning, a notice and null because you canot bind an static closure to an object

 ```


#### Attempting to bind an object to a static anonymous function


 ```php
<?php

$b = static function() { var_dump($this); };

$d = $b->bindTo(new StdClass());

$d(); //get a warning, a notice and null because you canot bind an static closure to an object

 ```



####  type hinting closures as Callables


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

#### Accessing private method of a class inside a Closure

 ``` php

 <?php

 class Scope
{

    private $property = 'default';

    public function run()
    {
        $self = $this;
        $func = function() use ($self) {
            $self->property = 'changed';
        };

        $func();
        var_dump($this->property);
    }
}

$scope = new Scope();
$scope->run();

//outputs 'changed'

 ```

#### Reflection class method getClosure

```php

class Example {
  static function printer () { echo "Hello World!\n"; }
}
 
$class = new ReflectionClass ('Example');
$method = $class->getMethod ('printer');
$closure = $method->getClosure ();
$closure ();


```


#### Closure::bind() example

```php

class A {
    private static $sfoo = 1;
    private $ifoo = 2;
}
$cl1 = static function() {
    return A::$sfoo;
};
$cl2 = function() {
    return $this->ifoo;
};

$bcl1 = Closure::bind($cl1, null, 'A');
$bcl2 = Closure::bind($cl2, new A(), 'A');

echo $bcl1(), "\n"; //outputs 1
echo $bcl2(), "\n"; //outputs 2

?>

``` 

####  Closure::bindTo() example

```php

<?php 

class A {
    function __construct($val) {
        $this->val = $val;
    }
    function getClosure() {
        //returns closure bound to this object and scope
        return function() { return $this->val; };
    }
}

$ob1 = new A(1);
$ob2 = new A(2);

$cl = $ob1->getClosure();
echo $cl(), "\n"; //outputs 1

$cl = $cl->bindTo($ob2);
echo $cl(), "\n"; //outputs 2

``` 

####  Closure call method usage example

```php

<?php 

class Value {
    protected $value;

    public function __construct($value) {
        $this->value = $value;
    }

    public function getValue() {
        return $this->value;
    }
}

$three = new Value(3);
$four = new Value(4);

$closure = function ($delta) { var_dump($this->getValue() + $delta); };

$closure->call($three, 4); //outputs int(7)

$closure->call($four, 4);  //outputs int(8)

``` 

# Tricks, tips and crazy things

#### closure as a value of an associative array


 ```php
<?php

$array['func'] = function(){
echo "hello";
};

$array['func'](); // hello


//WILL NOT WORK
$array = Array(
  'key' => function() { return $whatever; }
);

 ```

#### Immediatily invoking an anonimous function


 ```php
<?php

(function() { echo 123; })(); // will print 123


//Passing parameters

(function($name){
        echo 'My name is ' . $name;
    })('Wu Xiancheng');

 ```

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

#### using anonymous function to change a method at runtime


 ```php
<?php

class t
{
    var $num;

    var $dynamic_function;

    public function dynamic_function()
    {
        $func = $this->dynamic_function;

        $func($this);
    }
}

$p = new t();

$p->num = 5;

$p->dynamic_function = function($this_ref) // param cannot be named $this
{
    echo $this_ref->num++.'<br />';
};

$p->dynamic_function(); // CALL YOUR DYNAMIC FUNCTION

$p->dynamic_function = function($this_ref) // NEW DYNAMIC fUNCRION
{
    echo $this_ref->num.'<br />';

    $this_ref->num *= 3;
};

$p->dynamic_function(); // CALL DYNAMIC FUNCTION

echo $p->num;

//outputs
5
6
18

 ```


#### can't access array element inside use keyword


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


#### How to call a closure stored in a instance variable


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

#### $this variable is undefined inside static function


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

#### Adding public functions to class using closures


 ```php

 <?php
/*
    (string) $name Name of the function that you will add to class.
    Usage : $Foo->add(function(){},$name);
    This will add a public function in Foo Class.
    */
    class Foo
    {
        public function add($func,$name)
        {
            $this->{$name} = $func;
        }
        public function __call($func,$arguments){
            call_user_func_array($this->{$func}, $arguments);
        }
    }
    $Foo = new Foo();

    $Foo->add(function(){
        echo "Hello World";
    },"helloWorldFunction");

    $Foo->add(function($parameterone){
        echo $parameterone;
    },"exampleFunction");

    $Foo->helloWorldFunction(); /*Output : Hello World

    $Foo->exampleFunction("Hello PHP"); /*Output : Hello PHP*/

 ```

#### how to use closures to implement a Python-like decorator

  ```php

  <?php

/*
* An example showing how to use closures to implement a Python-like decorator
* pattern.
*
* My goal was that you should be able to decorate a function with any
* other function, then call the decorated function directly:
*
* Define function:         $foo = function($a, $b, $c, ...) {...}
* Define decorator:        $decorator = function($func) {...}
* Decorate it:             $foo = $decorator($foo)
* Call it:                 $foo($a, $b, $c, ...)
*
* This example show an authentication decorator for a service, using a simple
* mock session and mock service.
*/

session_start();

/*
* Define an example decorator. A decorator function should take the form:
* $decorator = function($func) {
*     return function() use $func) {
*         // Do something, then call the decorated function when needed:
*         $args = func_get_args($func);
*         call_user_func_array($func, $args);
*         // Do something else.
*     };
* };
*/
$authorise = function($func) {
    return function() use ($func) {
        if ($_SESSION['is_authorised'] == true) {
            $args = func_get_args($func);
            call_user_func_array($func, $args);
        }
        else {
            echo "Access Denied";
        }
    };
};

/*
* Define a function to be decorated, in this example a mock service that
* need to be authorised.
*/
$service = function($foo) {
    echo "Service returns: $foo";
};

/*
* Decorate it. Ensure you replace the origin function reference with the
* decorated function; ie just $authorise($service) won't work, so do
* $service = $authorise($service)
*/
$service = $authorise($service);

/*
* Establish mock authorisation, call the service; should get
* 'Service returns: test 1'.
*/
$_SESSION['is_authorised'] = true;
$service('test 1');

/*
* Remove mock authorisation, call the service; should get 'Access Denied'.
*/
$_SESSION['is_authorised'] = false;
$service('test 2');

?>

  ```


#### Calling closures assigned to class properties as class methods

  ```php

  <?php

  class foo {

  public test;

  public function __construct(){
    $this->test = function($a) {
      print "$a\n";
    };
  }

  public function __call($method, $args){
    if ( $this->{$method} instanceof Closure ) {
      return call_user_func_array($this->{$method},$args);
    } else {
      return parent::__call($method, $args);
    }
  }
}
$f = new foo();
$f->test();

  ```

#### can't use $this in use keyword


```php
<?php
$func = function() use ($this) {
    $this->property = 'changed';
};

//gives you 'PHP Fatal error:  Cannot use $this as lexical variable'


?>

```

#### check whether you're dealing with a closure specifically

```php
<?php

$isAClosure = is_callable($thing) && is_object($thing);

?>

```

#### Adding method on the fly to objects using the Closure::bind method

```php
<?php

trait MetaTrait
{
    
    private $methods = array();

    public function addMethod($methodName, $methodCallable)
    {
        if (!is_callable($methodCallable)) {
            throw new InvalidArgumentException('Second param must be callable');
        }
        $this->methods[$methodName] = Closure::bind($methodCallable, $this, get_class());
    }

    public function __call($methodName, array $args)
    {
        if (isset($this->methods[$methodName])) {
            return call_user_func_array($this->methods[$methodName], $args);
        }

        throw RunTimeException('There is no method with the given name to call');
    }

}


class HackThursday {
    use MetaTrait;

    private $dayOfWeek = 'Thursday';

}

$test = new HackThursday();
$test->addMethod('when', function () {
    return $this->dayOfWeek;
});

echo $test->when();

```

#### validate whether or not a closure can be bound to a PHP object

```php

/**
* @param \Closure $callable
*
* @return bool
*/
function isBindable(\Closure $callable)
{
    $bindable = false;

    $reflectionFunction = new \ReflectionFunction($callable);
    if (
        $reflectionFunction->getClosureScopeClass() === null
        || $reflectionFunction->getClosureThis() !== null
    ) {
        $bindable = true;
    }

    return $bindable;
}

```

#### using Closure BindTo method to create a small template engine

```php

class Article{
    private $title = "This is an article";
}

class Post{
    private $title = "This is a post";
}

class Template{

    function render($context, $tpl){

        $closure = function($tpl){
            ob_start();
            include $tpl;
            return ob_end_flush();
        };

        $closure = $closure->bindTo($context, $context);
        $closure($tpl);

    }

}

$art = new Article();
$post = new Post();
$template = new Template();

$template->render($art, 'tpl.php');
$template->render($post, 'tpl.php');
?>


#############
tpl.php
############
<h1><?php echo $this->title;?></h1>

```



# Resources

 * [Anonymous functions from PHP.net ](http://it2.php.net/manual/en/functions.anonymous.php)
 * [Closures from PHP.net ](http://www.php.net//manual/en/class.closure.php)
 * [Question about closures from an article on toptal.com](https://www.toptal.com/php)
 * [What are PHP Lambdas and Closures?] (http://culttt.com/2013/03/25/what-are-php-lambdas-and-closures/)
 * [Functional PHP] (https://leanpub.com/functional-php/read)
 * [https://wiki.php.net/rfc/closures](https://wiki.php.net/rfc/closures)
