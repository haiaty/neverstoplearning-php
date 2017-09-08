As of PHP 5.4.0, PHP implements a method of code reuse called Traits.

Traits[example](#trait-example) are a mechanism for **code reuse** in single inheritance languages such as PHP. 

A Trait is similar to a class, but only intended **to group functionality**
in a fine-grained and consistent way. It is not possible to instantiate a Trait on its own. 
It is an addition to traditional inheritance and enables horizontal composition of behavior; 
that is, the application of class members without requiring inheritance.

Multiple Traits can be inserted into a class by listing them in the use statement, separated by commas. [example](#)

Methods on the class that use the trait override  those on the trait. [example](#method-overriden-example)
but chhildren classes get their methods overriden by trait's methods [example](#children-method-overriden-example)


# Examples

##### Trait example

```php
<?php

trait Hello {
    public function sayHello() {
       echo "Hello from trait"
    }
}

class Example {

 use Hello;

}

$o = new Example();
$o->sayHello();
?>

//output is: Hello from trait



```


##### Methods on class override trait's method

```php
<?php

trait Hello {
    public function sayHello() {
       echo "Hello from trait"
    }
}

class Example {

 use Hello;

 public function sayHello() {
        echo 'Hello ';
    }
}

$o = new Example();
$o->sayHello();

?>

//output is: Hello



```

##### Methods on children class are overriden by trait's method

```php
<?php

trait Hello {
    public function sayHello() {
       echo "Hello from trait"
    }
}

class Example {

 use Hello;

 public function sayHello() {
        echo 'Hello ';
    }
}

class Children extends Example {
 use Hello;
}

$o = new Children();
$o->sayHello();

?>

//output is: Hello from trait



```