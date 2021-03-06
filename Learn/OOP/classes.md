
* [summary](#class)
* [examples](#examples)
* [resources](#resources)

# Class

Basic class definitions begin with the keyword class, followed by a class name, followed by a pair of curly braces which enclose the definitions of the properties and methods belonging to the class.

The class name can be any valid label, provided it is not a PHP reserved word. A valid class name starts with a letter or underscore, followed by any number of letters, numbers, or underscores. As a regular expression, it would be expressed thus: ^[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*$.

A class may contain its own constants, variables (called "properties"), and functions (called "methods"). ([source](http://php.net/manual/en/language.oop5.basic.php))


Classes can be considered as a collection of methods, variables and constants. They often reflect a real-world thing, like a Car class or a Fruit class. You declare a class only once, but you can instantiate as many versions of it as can be contained in memory. An instance of a class is usually referred to as an object. ([source](http://www.php5-tutorial.com/classes/introduction/))

---

# Examples

### simple class definition

```php
<?php

class Car {

}

```

### class definition with properties

```php
<?php

class Car {

    /*
    * @var string licensePlate
    */
    public $licensePlate;

    /*
    * @var int $numberOfWheels
    */
    private $numberOfWheels;

    /*
    * @var string $color
    */

    protected $color = 'blue';

}

```
### class definition with methods

```php
<?php

 class Car {

    public $wheels;

    /*
    * Starts the car 
    */
    public function startEngine()
    {
        //
    }

}

```




# resources

* [Class definition from official php site](http://php.net/manual/en/language.oop5.basic.php)
* [Classes definition from Hacking PHP site](http://www.hackingwithphp.com/6/2/0/classes)
