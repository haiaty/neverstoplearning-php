
Some variables definitions

```php

$txt = "Hello world!";
$x = 5;
$y = 10.5;

$4site = 'not yet';     // invalid; starts with a number
$_4site = 'not yet';    // valid; starts with an underscore
$täyte = 'mansikka';    // valid; 'ä' is (Extended) ASCII 228.

```

---

Values assignment by reference

```php

$foo = 'Bob';              // Assign the value 'Bob' to $foo

$bar = &$foo;   //now $bar has a reference to $foo. If the value of $foo changes, $bar will have the new value

```

