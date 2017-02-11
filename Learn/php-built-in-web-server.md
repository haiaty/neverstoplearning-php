

* [knowledge aggregation](#phps-built-in-web-server)
* [Resources](#resources)


# PHP's Built in web server

The web server runs a only one **single-threaded process**, so PHP applications will stall if a request is blocked.

URI **requests are served from the current working directory** where PHP was started, unless the -t option is used to specify an explicit document root. If a URI request does not specify a file, then either index.php or index.html in the given directory are returned. If neither file exists, the lookup for index.php and index.html will be continued in the parent directory and so on until one is found or the document root has been reached. If an index.php or index.html is found, it is returned and $_SERVER['PATH_INFO'] is set to the trailing part of the URI. Otherwise a 404 response code is returned.

If a PHP file is given on the command line when the web server is started it is treated as a "router" script. The script is run at the start of each HTTP request. If this script returns FALSE, then the requested resource is returned as-is. Otherwise the script's output is returned to the browser.


---

# Resources

* [PHP s built in web server documentation on PHP net](http://php.net/manual/en/features.commandline.webserver.php)
