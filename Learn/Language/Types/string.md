
* [Examples](#examples)
  * [Use of sintax syntax inside strings](#use-of-sintax-syntax-inside-strings)
  * [Use of complex curly syntax inside strings](#use-of-complex-curly-syntax-inside-strings)





#### Complex (curly) syntax

This sintax allows the use of complex expressions inside strings ([Examples](#use-of-complex-curly-syntax-inside-strings)).

Any scalar variable, array element or object property with a string representation can be included via this syntax. Simply write the expression the same way as it would appear outside the string, and then wrap it in { and }. Since { can not be escaped, this syntax will only be recognised when the $ immediately follows the {.

Functions, method calls, static class variables, and class constants inside {$} work since PHP 5. However, **the value accessed will be interpreted as the name of a variable in the scope** in which the string is defined. Using single curly braces ({}) **will not work for accessing the return values of functions or methods or the values of class constants or static class variables**.

this syntax is unnecessary if you don't need complex expressions but only the value of the variable. You can use the simple sintax([Example](#use-of-sintax-syntax-inside-strings))




# Examples

### Use of sintax syntax inside strings

 ```php
 
 $juice = "apple";

echo "He drank some $juice juice.";

 
  ```

### Use of complex curly syntax inside strings

 ```php

 <?php

// Show all errors
error_reporting(E_ALL);

$great = 'fantastic';

// Won't work, outputs: This is { fantastic}
echo "This is { $great}";

// Works, outputs: This is fantastic
echo "This is {$great}";

// Works
echo "This square is {$square->width}00 centimeters broad."; 


// Works, quoted keys only work using the curly brace syntax
echo "This works: {$arr['key']}";


// Works
echo "This works: {$arr[4][3]}";

// This is wrong for the same reason as $foo[bar] is wrong  outside a string.
// In other words, it will still work, but only because PHP first looks for a
// constant named foo; an error of level E_NOTICE (undefined constant) will be
// thrown.
echo "This is wrong: {$arr[foo][3]}"; 

// Works. When using multi-dimensional arrays, always use braces around arrays
// when inside of strings
echo "This works: {$arr['foo'][3]}";

// Works.
echo "This works: " . $arr['foo'][3];

echo "This works too: {$obj->values[3]->name}";

echo "This is the value of the var named $name: {${$name}}";

echo "This is the value of the var named by the return value of getName(): {${getName()}}";

echo "This is the value of the var named by the return value of \$object->getName(): {${$object->getName()}}";

// Won't work, outputs: This is the return value of getName(): {getName()}
echo "This is the return value of getName(): {getName()}";

class foo {
    var $bar = 'I am bar.';
}

$foo = new foo();
$bar = 'bar';
$baz = array('foo', 'bar', 'baz', 'quux');
echo "{$foo->$bar}\n"; //outputs 'I'm bar'
echo "{$foo->{$baz[1]}}\n"; //outputs 'I'm bar'


?>

 ```
