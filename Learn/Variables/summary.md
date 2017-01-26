In PHP, a variable starts with the $ sign, followed by the name of the variable.

Rules for PHP variables:

* A variable starts with the $ sign, followed by the name of the variable
* A variable name must start with a letter or the underscore character
* A variable name cannot start with a number
* A variable name can only contain alpha-numeric characters and underscores (A-z, 0-9, and _ )
* Variable names are case-sensitive ($age and $AGE are two different variables)  

As a regular expression, it would be expressed thus: '[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*'

By default, variables are always assigned by value. That is to say, when you assign an expression to a variable, the entire value of the original expression is copied into the destination variable. This means, for instance, that after assigning one variable's value to another, changing one of those variables will have no effect on the other.

PHP also offers another way to assign values to variables: assign by reference. This means that the new variable simply references (in other words, "becomes an alias for" or "points to") the original variable. Changes to the new variable affect the original, and vice versa.
To assign by reference, simply prepend an ampersand (&) to the beginning of the variable which is being assigned (the source variable). 

Sources:

* [http://www.w3schools.com/php/php_variables.asp](http://www.w3schools.com/php/php_variables.asp)
* [http://php.net/manual/en/language.variables.basics.php](http://php.net/manual/en/language.variables.basics.php)
