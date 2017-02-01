
* [summary](#with-web-server)
* [examples](#examples)
* [resources](#resources)


# With Web Server

The Web server will handle the request coming from client and then will pass it to the PHP interpreter.

The server should activate support for PHP and all files ending in .php should be handled by PHP. On most servers, this is the default extension for PHP files. If your server supports PHP, then you do not need to do anything. Just create your .php files, put them in your web directory and the server will automatically parse them for you. There is no need to compile anything nor do you need to install any extra tools.

The server finds out that this file needs to be interpreted by PHP because you used the ".php" extension, which the server is configured to pass on to PHP. 


# From Cli (Command line interface)

PHP should be installed on the server. When it is installed you will have the 'php' command in the shell. You can both execute the php in interactive mode from shell or execute a file having the '.php' extension.

---

# Examples

#### Using web server

given this hello.php file:

```php

<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <?php echo '<p>Hello World</p>'; ?> 
 </body>
</html>

```

Use your browser to access the file with your web server's URL, ending with the /hello.php file reference. When developing locally this URL will be something like http://localhost/hello.php or http://127.0.0.1/hello.php but this depends on the web server's configuration. If everything is configured correctly, this file will be parsed by PHP and the following output will be sent to your browser:

```html
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <p>Hello World</p>
 </body>
 ```

---

# Resources

* [Simple tutorial from php.net](http://php.net/manual/en/tutorial.firstpage.php)
