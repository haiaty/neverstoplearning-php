
* [summary](#variables)
* [examples](#examples)
* [resources](#resources)

# Variables

---

### :bulb: **How to**
 
 In PHP, a variable starts with the $ sign, followed by the name of the variable.
 
--

### :bulb: **rules for variables**
 
Rules for PHP variables:
 
 * A variable starts with the $ sign, followed by the name of the variable
 * A variable name must start with a letter or the underscore character
 * A variable name cannot start with a number
 * A variable name can only contain alpha-numeric characters and underscores (A-z, 0-9, and _ )
 * Variable names are case-sensitive ($age and $AGE are two different variables)  
 
As a regular expression, it would be expressed thus: '[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*'
 
--
 
### :bulb: **By default variables are always assigned by value**
 
By default, variables are always assigned by value. That is to say, when you assign an expression to a variable, the entire value of the original expression is copied into the destination variable. This means, for instance, that after assigning one variable's value to another, changing one of those variables will have no effect on the other.
 
-- 
 
### :bulb: **you can also assign by reference**
 
PHP also offers another way to assign values to variables: assign by reference. This means that the new variable simply references (in other words, "becomes an alias for" or "points to") the original variable. Changes to the new variable affect the original, and vice versa.
To assign by reference, simply prepend an ampersand (&) to the beginning of the variable which is being assigned (the source variable). 



# Examples

---

Some variables definitions
 
 ```php
 
 $txt = "Hello world!";
 $x = 5;
 $y = 10.5;
 
 $4site = 'not yet';     // invalid; starts with a number
 $_4site = 'not yet';    // valid; starts with an underscore
 $täyte = 'mansikka';    // valid; 'ä' is (Extended) ASCII 228.
 
 ```
 
 
 Values assignment by reference
 
 ```php
 
 $foo = 'Bob';              // Assign the value 'Bob' to $foo
 
 $bar = &$foo;   //now $bar has a reference to $foo. If the value of $foo changes, $bar will have the new value
 
 ```
 
 
  Name of a variable generated from the value of another variables.
  
  (Note that this will work only if the value is a string).
 
 ```php
 
 $foo = 'Bob';              // Assign the value 'Bob' to $foo
 
 $$foo = 'Hello'; //we we have created a variable $Bob that has the string 'Hello' as value
 
 echo $Bob; // 'Hello'
 
 ```




# Resources

---

* [Variables from PHP.Net ](http://php.net/manual/en/language.variables.php)
* [Variables from W3School](http://www.w3schools.com/php/php_variables.asp)
* [Variable comparision cheat sheet] (http://phpcheatsheets.com/compare/)

