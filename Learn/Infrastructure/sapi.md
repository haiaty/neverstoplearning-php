

#SAPIs – different runtime environments of PHP

# What is an SAPI?

SAPI stands for "Server Application Programming Interface". It is the mechanism that controls the interaction between the "outside world" and the PHP/Zend engine .

it is the **direct module interface to web servers** such as the Apache HTTP Server, Microsoft IIS, and Oracle iPlanet Web Server. Microsoft uses the term Internet Server Application Programming Interface (ISAPI), and the defunct Netscape web server used the term Netscape Server Application Programming Interface (NSAPI) for the same purpose.In other words, SAPI is an application programming interface (API) provided by the web server to help other developers in extending the web server capabilities.

If you want to know the type of SAPI that PHP is using you can use the [php_sapi_name](http://php.net/manual/en/function.php-sapi-name.php) function. An alternative approach is to use the the PHP constant PHP_SAPI since it has the same value as php_sapi_name().


## mod_php NTS

(non-thread safety) this SAPI integrates into Apache Httpd (2.2.* on RHEL/CentOS 6, 2.4.* on RHEL/CentOS 7). It is the standard SAPI for use with httpd prefork mpm (the default mode httpd is ran under. It is not thread-safe, but doesn’t need to be due to prefork not using threads.

## CLI

this SAPI allows running scripts from the command-line, and also has a built-in web server for development-use. Located at /usr/bin/php

## FPM

(FastCGI Process Manager) is a scalable FastCGI process, which acts similar to how Httpd prefork mpm works managing it’s forks. Located at 

## phpdbg

phpdbg has the ability to debug scripts using breakpoints from the command-line, and also supports remote-debugging using an external Java client for remote communication.

## embedded

this SAPI allows embedding PHP in other applications. It’s library is located at /usr/lib[64]/libphp7.so

## cgi, fastcgi
these SAPIs are not recommended for use, but are available where needed. They both exist in the binary at /usr/bin/php-cgi.

## mod_php TS
(thread safety)this SAPI integrates into Apache Httpd (2.2.* on RHEL/CentOS 6, 2.4.* on RHEL/CentOS 7). It is the standard SAPI for use with httpd worker mpm. It’s supposed to be thread-safe, but can’t guarantee to be, and certainly not under additional PHP extensions. It’s better to use FastCGI SAPIs than this one. It’s located at /usr/lib[64]/httpd/modules/libphp7-zts.so

---

# Resources

* 
