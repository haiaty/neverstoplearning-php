
* [knowledge aggregation](#php-fpm)
* [resources](#resources)

# PHP FPM

FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features (mostly) useful for heavy-loaded sites.


### How it works

-> PHP-FPM organizes it's processes in so-called pools

-> Each pool is represented by a master process that is listening on a particular socket (network or unix domain socket). 

-> The master process queues incoming requests, spawns child processes in background, distributes the workload to its child processes and writes the responses back to the socket/stream as soon as the child processes finished and handed over their results.

--> PHP-FPM knows three different types for the process management (config value for pm):

**dynamic** (as shown above)
You can configure how many child processes are always alive to immediately handle requests without an upstart time. This should be used when performance and quick responses are the focus of the application. That's why it is commonly used in combination with a webserver like nginx.

**ondemand** (see example below)
The configured amount of child processes will be started (only) as soon as the master process gets requests. After a certain amount of time these child processes will die and only the master process stays alive.
I personally prefer this mode for the background workers I described in my blog post, because the quick response is not that important in this use-case. Depending on the workload you don't want to have a lot of idle processes consuming resources for nothing.

**static**
You configure a fixed number of child processes. Until now I did't see a use-case for this.

### Features

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



### check how many request server can handle

It’s good to test how many PHP-FPM processes a server could handle easily, first start Nginx and PHP-FPM and load some PHP pages, preferably all of the heaviest pages. Then check memory usage per PHP-FPM process example with Linux top or htop command. Let’s assume that the server has 512 Mb memory and 220 Mb could be used for PHP-FPM, every process use 24 Mb RAM (some huge content management system with plugins can easily use 20-40 Mb / per PHP page request or even more). Then simply calculate the server max_children value:
220 / 24 = 9.17

So good pm.max_children value is 9. This is based just quick average and later this could be something else when you see longer time memory usage / per process. After quick testing it’s much easier to setup pm.start_servers value, pm.min_spare_servers value and pm.max_spare_servers value.

**max_children** is the amount of spawned processes the fpm is allowed to spawn to handle concurrent connections, If you are having a lot of concurrent connections then you better increase that number, if the number is reached fpm won't spawn another child and wait for one to free to handle the waiting request.

Max request per process is unlimited by default, but it’s good to set some low value, like 200 and avoid some memory issues. 


### Other informations

--> You can have any number of “pools” you like. They will each maintain their own set of processes and live in their own little sandbox. For most setups there’s no reason to have more than one (especially if you are the only one hosting sites on the server), so we’ll configure a single global pool called “www” — just like the default. With PHP-FPM it’s possible to use different pools for different sites and allocate resources very accurately and even use different users and groups for every pool.

--> The advantage of running PHP-FPM on socket connections instead of TCP/IP is that the socket connections are much more faster than TCP/IP connections (around 10-15%) because it saves the passing the data over the different layers of TCP/IP stack.

Therefore, it is recommended to run the PHP-FPM on socket connections over TCP/IP when you are using the same server for your web server and PHP-FPM. If you are using the different servers for your web server and PHP-FPM then the socket connections for PHP-FPM will not work. However if you use PHP-FPM on socket connections maybe you could get **errors of the type "Resource temporary not available"** in the NGINX logs (or you webserver logs). If you are getting this error you should change to TCP/IP.

--> PHP-FPM is FAST - but be wary of using it while your code base is stored on NFS - under average load your NFS server will feel some serious strain. 

### tips

Best way to use PHP-FPM process manager is use dynamic process management, so PHP-FPM processes are started only when needed. This is almost same style setup than Nginx worker_processes and worker_connections setup. So very high values does not mean necessarily anything good. Every process eat memory and of course if site have very high traffic and server lot’s of memory then higher values are right choise, but servers, like VPS (Virtual Private Servers) memory is normally limited to 256 Mb, 512 Mb, 1024 Mb. This low RAM is enough to handle even very high traffic (even dozens of requests per second), if it’s used wisely.


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


## Handling errors

### [pool www] child 84583 exited on signal 11 (SIGSEGV)

The first possible solution has to do with session files. If somehow the directory where they ought to be stored (session.save_path = "/var/lib/php/sessions", by default) is not writable, then php_fpm will segfault (note: 'not writable' might mean a lot of possible issues, from wrong permissions/ownership, wrong user/group running php_fpm, or, more obscurely, hitting inode limits on the filesystem where the save_path directory is mounted...). This is usually easy to fix — just check and re-check if the directory has all the permissions for the correct user; if it's full of session files, it's possible that the PHP garbage collector is not running; you can delete everything and see if it helps, or move the directory to a different mount point, etc. 

The second possible solution is even more cryptical and obscure, and is tied to bad/buggy extensions. The original poster mentions that he uses apc and memcache. Trying to run without either of those extensions is a possibility (in my own case, it was the extension installed by New Relic that caused the problem). What usually happens in these cases is that those extensions sometimes hit some kind of limit (or have a bug which does not manifest itself all the time, and has eluded the testers), they segfault and bring the php-fpm process down with them — leaving no traces on logs, no core files around, nothing but the annoying 'child XXX exited on signal 11', making it very hard to debug, especially because it does not happen always.  To debug you shold use dbg.


---

# Resources

- [FastCGI Process Manager (FPM) ](http://php.net/manual/en/install.fpm.php)
- [Get High Performance PHP-FPM with socket connections](http://voidweb.com/2010/10/get-high-performance-php-fpm-socket-connections/)
- [Enable PHP-FPM Status ](https://easyengine.io/tutorials/php/fpm-status-page/)
- [Nginx and PHP-FPM Configuration and Optimizing Tips and Tricks](https://www.if-not-true-then-false.com/2011/nginx-and-php-fpm-configuration-and-optimizing-tips-and-tricks/)
- [Q/A: Parallelism, Experimental async PHP - VOL. 2](https://hollo.me/php/experimental-async-php-volume-2-parallelism.html)
