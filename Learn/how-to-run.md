
* [knowledge aggregation](#with-web-server)
  * [with Web Server](#with-web-server)
    * [PHP built in web server](#with-php-built-in-web-server)
    * [Apache web server](#with-apache-web-server)
  * [From Cli (Command line interface)](#from-cli-command-line-interface)
  * [PHP for desktop applications](#php-for-desktop-applications)
* [examples](#examples)
  * [Running PHP with web server](#running-php-with-web-server)
* [resources](#resources)


# With Web Server


The Web server will handle the request coming from client and then will pass it to the PHP interpreter.

The web server finds out that this file needs to be interpreted by PHP because you used the ".php" extension.

You have **two choices** for the method of connecting PHP to the web server. For many web servers **PHP has a direct module interface (also called SAPI)**. These web servers include Apache, Microsoft Internet Information Server, Netscape and iPlanet servers. Many other web servers have support for ISAPI, the Microsoft module interface (OmniHTTPd for example). If PHP **has no module support** for your web server, you can always **use it as a CGI or FastCGI processor**. 


### with PHP built in web server

As of PHP 5.4.0, the CLI SAPI provides a built-in web server. Note that this web server was designed to aid application development. It may also be useful for testing purposes or for application demonstrations that are run in controlled environments. It is not intended to be a full-featured web server. It should not be used on a public network.

For more information about it see here: [PHP's built in web server](https://github.com/haiaty/neverstoplearning-php/blob/master/Learn/php-built-in-web-server.md)



### with Apache Web server

The traditional way is to use Apache’s mod_php. Mod_php attaches PHP to Apache itself, but Apache does a very bad job of managing it. You’ll suffer from severe memory problems as soon as you get any kind of real traffic.
Two new options soon became popular: mod_fastcgi and mod_fcgid. Both of these keep a limited number of PHP processes running, and Apache sends requests to these interfaces to handle PHP execution on its behalf. Because these libraries limit how many PHP processes are alive, memory usage is greatly reduced without affecting performance.
Some smart people created an implementation of fastcgi that was specially designed to work really well with PHP, and they called it PHP-FPM.

**Apache + mod_php**: If configured with mod_php, Apache starts a new PHP instance each time it receives a request to serve a PHP script, then destroys the instance when the script is done. this setup is quite performance intensive and has some drawbacks which can be seen when the site grows in terms of users. Apache require more RAM to serve the requests whereas mod_php results in increased CPU cycles. The single user nature of PHP adds more complications because each user will have its own php process running on the webserver any time of request.

**Apache + mod_fastcgi**: FastCGI is a module that allows you to neatly solve mod_php’s big problem, namely that it must spin up and destroy a PHP instance with every request. FastCGI instead keeps an instance of PHP running in the background. When Apache receives a request it forwards it to FastCGI, which feeds it to its already running instance of PHP and sends the result back to Apache. Apache then serves the result.Without the constant build-and-destroy of new PHP processes, FastCGI is a great memory saver and performance booster. 

**Apache + mod_fcgid**:  fgcid is a binary-compatible alternative to FastCGI–that is, it does more or less the same thing, but in a different way. It seems that some people prefer mod_fcgid over mod_fastcgi because of better stability and maybe even slightly better performance. 

**Apache + PHPFPM**: With php-fpm, the FPM process keeps a certain number of PHP instances alive at all times, and Apache sends requests to those living instances instead of starting and destroying a new one with each request.

# From Cli (Command line interface)

In this case, you need no server and no browser.PHP should be installed on the server. you need the command line executable. When it is installed you will have the 'php' command in the shell. You can both execute the php in interactive mode from shell or execute a file having the '.php' extension.

# PHP for desktop applications

With PHP you can also write desktop GUI applications using the PHP-GTK extension. This is a completely different approach than writing web pages, as you do not output any HTML, but manage windows and objects within them.PHP-GTK is not included in the official PHP distribution.

---

# Examples

#### Running PHP with web server

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
* [Serving PHP from Apache using PHP-FPM](https://phpbestpractices.org/#serving-php)
* [The perrfect lamp stack](http://voidweb.com/2010/07/the-perfect-lamp-stack-apache2-fastcgi-php-fpm-apc/)
