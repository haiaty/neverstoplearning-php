
* [summary](#with-web-server)
* [examples](#examples)
* [how to](#how-to)
* [resources](#resources)


# With Web Server

The Web server will handle the request coming from client and then will pass it to the PHP interpreter.

The server should activate support for PHP and all files ending in .php should be handled by PHP. On most servers, this is the default extension for PHP files. If your server supports PHP, then you do not need to do anything. Just create your .php files, put them in your web directory and the server will automatically parse them for you. There is no need to compile anything nor do you need to install any extra tools.

The server finds out that this file needs to be interpreted by PHP because you used the ".php" extension, which the server is configured to pass on to PHP. 

### with Apache Web server

The traditional way is to use Apache’s mod_php. Mod_php attaches PHP to Apache itself, but Apache does a very bad job of managing it. You’ll suffer from severe memory problems as soon as you get any kind of real traffic.
Two new options soon became popular: mod_fastcgi and mod_fcgid. Both of these keep a limited number of PHP processes running, and Apache sends requests to these interfaces to handle PHP execution on its behalf. Because these libraries limit how many PHP processes are alive, memory usage is greatly reduced without affecting performance.
Some smart people created an implementation of fastcgi that was specially designed to work really well with PHP, and they called it PHP-FPM.

**Apache + mod_fastcgi**: FastCGI is a module that allows you to neatly solve mod_php’s big problem, namely that it must spin up and destroy a PHP instance with every request. FastCGI instead keeps an instance of PHP running in the background. When Apache receives a request it forwards it to FastCGI, which feeds it to its already running instance of PHP and sends the result back to Apache. Apache then serves the result.Without the constant build-and-destroy of new PHP processes, FastCGI is a great memory saver and performance booster. 

**Apache + mod_fcgid**:  fgcid is a binary-compatible alternative to FastCGI–that is, it does more or less the same thing, but in a different way. It seems that some people prefer mod_fcgid over mod_fastcgi because of better stability and maybe even slightly better performance. 

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

# How to

### Install and configure Apache 2 and PHP-FPM on Ubuntu 14

You can install PHP-FPM and Apache on Ubuntu 14.04 by running these command in your terminal:

```
sudo apt-get install apache2-mpm-event libapache2-mod-fastcgi php5-fpm
sudo a2enmod actions alias fastcgi

```

Note that we must use apache2-mpm-event (or apache2-mpm-worker), not apache2-mpm-prefork or apache2-mpm-threaded.

Next, we’ll configure our Apache virtualhost to route PHP requests to the PHP-FPM process. Place the following in your Apache configuration file (in Ubuntu 14.04 the default one is /etc/apache2/sites-available/000-default.conf).

```
<Directory />
    Require all granted
</Directory>
<VirtualHost *:80>
    Action php5-fcgi /php5-fcgi
    Alias /php5-fcgi /usr/lib/cgi-bin/php5-fcgi
    FastCgiExternalServer /usr/lib/cgi-bin/php5-fcgi -socket /var/run/php5-fpm.sock -idle-timeout 120 -pass-header Authorization
    <FilesMatch "\.php$">
        SetHandler  php5-fcgi
    </FilesMatch>
</VirtualHost>
```

Finally, restart Apache and the FPM process:

```
sudo service apache2 restart && sudo service php5-fpm restart

```

# Resources

* [Simple tutorial from php.net](http://php.net/manual/en/tutorial.firstpage.php)
* [Serving PHP from Apache using PHP-FPM](https://phpbestpractices.org/#serving-php)
* [Installing Apache + MOD_FASTCGI + PHP-FPM on ubuntu server](https://alexcabal.com/installing-apache-mod_fastcgi-php-fpm-on-ubuntu-server-maverick/)
