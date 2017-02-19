
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

PHP-FPM is FAST - but be wary of using it while your code base is stored on NFS - under average load your NFS server will feel some serious strain. 

### how to enable status page for FPM and NGINX

In php-fpm config

```shell

vi /etc/php-fpm.d/www.conf

```
Search for the status path directive and enable it
```shell

pm.status_path = /status

```
Then make sure nginx can call this location. In you nginx site config

```shell

vi /etc/nginx/conf.d/mysite.conf

Add

location ~ ^/(status|ping)$ {
     access_log off;
     #allow 127.0.0.1;
     #allow 1.2.3.4#your-ip;
     #deny all;
     include fastcgi_params;
     fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
     fastcgi_pass 127.0.0.1:9000;
 }
 ```
Notice above i have commented out the allow and deny instructions to have the status page enabled from any IP. Make sure this is not enabled on productin. Now restart both nginx and php-fpm

```shell
sudo service nginx restart
sudo service php-fpm restart

```

##### output explanation

Below is meaning of different values

- **pool** – the name of the pool. Mostly it will be www.
- **process manager** – possible values static, dynamic or ondemand. We never use static.  Trying ondemand is on todo list.
- **start time** – the date and time FPM has started or reloaded. Reloading PHP-FPM (service php5-fpm reload) reset this value.
- **start since** – number of seconds since FPM has started
- **accepted conn** – the number of request accepted by the pool
- **listen queue** – the number of request in the queue of pending connections. If this number is non-zero, then you better increase number of process FPM can spawn.
- **max listen queue** – the maximum number of requests in the queue of pending connections since FPM has started
- **listen queue len** – the size of the socket queue of pending connections
- **idle processes** – the number of idle processes
- **active processes** – the number of active processes
- **total processes** – the number of idle + active processes
- **max active processes** – the maximum number of active processes since FPM has started
- **max children reached** – number of times, the process limit has been reached, when pm tries to start more children. If that value is not zero, then you may need to increase max process limit for your PHP-FPM pool. Like this, you can find other useful information to tweak your pool better way.
-** slow requests ** – Enable php-fpm slow-log before you consider this. If this value is non-zero you may have slow php processes. Poorly written mysql queries are generally culprit.

#### output explanation full 
(Below is meaning of different values)

- **pid** – the PID of the process. You can use this PID to kill a long running process.
- **state** – the state of the process (Idle, Running, …)
- **start time** – the date and time the process has started
- **start since** – the number of seconds since the process has started
- **requests** – the number of requests the process has served
- **request duration** – the duration in µs of the requests
- **request method** – the request method (GET, POST, …)
- **request URI** – the request URI with the query string
- **content length** – the content length of the request (only with POST)
- **user** – the user (PHP_AUTH_USER) (or ‘-‘ if not set)
- **script **– the main PHP script called (or ‘-‘ if not set)
- **last request cpu** – the %cpu the last request consumed. it’s always 0 if the process is not in Idle state because CPU calculation is done when the request processing has terminated
- **last request memory **-the max amount of memory the last request consumed. it’s always 0 if the process is not in Idle state because memory calculation is done when the request processing has terminated

If the process is in Idle state, then informations are related to the last request the process has served. Otherwise informations are related to the current request being served.

---

# Resources

- [FastCGI Process Manager (FPM) ](http://php.net/manual/en/install.fpm.php)
- [Get High Performance PHP-FPM with socket connections](http://voidweb.com/2010/10/get-high-performance-php-fpm-socket-connections/)
- [Enable PHP-FPM Status ](https://easyengine.io/tutorials/php/fpm-status-page/)
