




If the directive error_log is not set on the php.ini file, errors are sent to the **SAPI error logger**. For example, it is an error log in Apache, an error log on nginx (so check the error log of the web server) or stderr in CLI.

If you are using PHP-FPM and you want to see the php errors on the fpm error log (the pool error log) you should add this configuration to the pool conf file (the pool file that you web server is using) of fpm:

```shell

catch_workers_output = yes

```


- [ error_log directive on PHP.net](http://php.net/manual/en/errorfunc.configuration.php#ini.error-log)
