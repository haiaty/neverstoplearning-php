

# PHP TAGS

When PHP parses a file, it looks for opening and closing tags, which are <?php and ?> which tell PHP to start and stop interpreting the code between them. 

Everything outside of a pair of opening and closing tags is ignored by the PHP parser.

PHP also allows for short open tag <? (which is discouraged).

If a file is pure PHP code, it is preferable to omit the PHP closing tag at the end of the file. Why? because this prevents accidental whitespace or new lines being added after the PHP closing tag, which may cause unwanted effects because PHP will start output buffering when there is no intention from the programmer to send any output at that point in the script.


# Examples


### php tag with new line
```php

<?php

echo "Hello world";

// ... more code

echo "Last statement";

// the script ends here with no PHP closing tag

```

### php tag in the same line with space
```php

<?php /*php followed by space*/ echo "a"?> //works

```

### php tag without space will not work

```php

<?php/*blah*/ echo "a"?>  //will not work

```


# SOURCES

[PHP tags from php.net](http://php.net/manual/en/language.basic-syntax.phptags.php)

