
* [summary](#php-fpm)
* [resources](#resources)

# PHP FPM

PHP-FPM not only supports TCP/IP connections but also the socket based connections.

The advantage of running PHP-FPM on socket connections instead of TCP/IP is that the socket connections are much more faster than TCP/IP connections (around 10-15%) because it saves the passing the data over the different layers of TCP/IP stack.

Therefore, it is recommended to run the PHP-FPM on socket connections over TCP/IP when you are using the same server for your web server and PHP-FPM. If you are using the different servers for your web server and PHP-FPM then the socket connections for PHP-FPM will not work.

---

# Resources

[Get High Performance PHP-FPM with socket connections](http://voidweb.com/2010/10/get-high-performance-php-fpm-socket-connections/)
