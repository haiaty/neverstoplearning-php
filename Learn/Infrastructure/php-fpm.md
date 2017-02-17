
* [knowledge aggregation](#php-fpm)
* [resources](#resources)

# PHP FPM

FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features (mostly) useful for heavy-loaded sites.

These features include:

- advanced process management with graceful stop/start;

- ability to start workers with different uid/gid/chroot/environment, listening on different ports and using different php.ini (replaces safe_mode);

- stdout and stderr logging;

- emergency restart in case of accidental opcode cache destruction;

- accelerated upload support;

- "slowlog" - logging scripts (not just their names, but their PHP backtraces too, using ptrace and similar things to read remote process' execute_data) that are executed unusually slow;

- fastcgi_finish_request() - special function to finish request and flush all data while continuing to do something time-consuming (video converting, stats processing etc.);

- dynamic/static child spawning;

- basic SAPI status info (similar to Apache mod_status);

- php.ini-based config file.

PHP-FPM not only supports TCP/IP connections but also the socket based connections.

The advantage of running PHP-FPM on socket connections instead of TCP/IP is that the socket connections are much more faster than TCP/IP connections (around 10-15%) because it saves the passing the data over the different layers of TCP/IP stack.

Therefore, it is recommended to run the PHP-FPM on socket connections over TCP/IP when you are using the same server for your web server and PHP-FPM. If you are using the different servers for your web server and PHP-FPM then the socket connections for PHP-FPM will not work.

---

# Resources

- [FastCGI Process Manager (FPM) ](http://php.net/manual/en/install.fpm.php)
- [Get High Performance PHP-FPM with socket connections](http://voidweb.com/2010/10/get-high-performance-php-fpm-socket-connections/)
