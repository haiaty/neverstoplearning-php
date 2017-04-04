

# php.ini

The configuration file (php.ini) is read when PHP starts up. For the server module versions of PHP, this happens only once when the web server is started. For the CGI and CLI versions, it happens on every invocation.

## Where php.ini file is searched

php.ini is searched for in these locations (in order):

* SAPI module specific location (PHPIniDir directive in Apache 2, -c command line option in CGI and CLI, php_ini parameter in NSAPI, PHP_INI_PATH environment variable in THTTPD)
* The PHPRC environment variable. Before PHP 5.2.0, this was checked after the registry key mentioned below.
* As of PHP 5.2.0, the location of the php.ini file can be set for different versions of PHP. The following registry keys are examined in order: [HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x.y.z], [HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x.y] and [HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x], where x, y and z mean the PHP major, minor and release versions. If there is a value for IniFilePath in any of these keys, the first one found will be used as the location of the php.ini (Windows only).[HKEY_LOCAL_MACHINE\SOFTWARE\PHP], value of IniFilePath (Windows only).
* Current working directory (except CLI).
* The web server's directory (for SAPI modules), or directory of PHP (otherwise in Windows).
* Windows directory (C:\windows or C:\winnt) (for Windows), or --with-config-file-path compile time option.

If php-SAPI.ini exists (where SAPI is the SAPI in use, so, for example, php-cli.ini or php-apache.ini), it is used instead of php.ini.

#### Scan dir

It is possible to configure PHP to scan for .ini files in a directory after reading php.ini. This can be done at compile time by setting the --with-config-file-scan-dir option. In PHP 5.2.0 and later, the scan directory can then be overridden at run time by setting the PHP_INI_SCAN_DIR environment variable.

It is possible to scan multiple directories by separating them with the platform-specific path separator (; on Windows, NetWare and RISC OS; : on all other platforms; the value PHP is using is available as the PATH_SEPARATOR constant). If a blank directory is given in PHP_INI_SCAN_DIR, PHP will also scan the directory given at compile time via --with-config-file-scan-dir .

Within each directory, PHP will scan all files ending in .ini in alphabetical order. A list of the files that were loaded, and in what order, is available by calling php_ini_scanned_files(), or by running PHP with the --ini option.

```php
Assuming PHP is configured with --with-config-file-scan-dir=/etc/php.d,
and that the path separator is :...

$ php
  PHP will load all files in /etc/php.d/*.ini as configuration files.

$ PHP_INI_SCAN_DIR=/usr/local/etc/php.d php
  PHP will load all files in /usr/local/etc/php.d/*.ini as
  configuration files.

$ PHP_INI_SCAN_DIR=:/usr/local/etc/php.d php
  PHP will load all files in /etc/php.d/*.ini, then
  /usr/local/etc/php.d/*.ini as configuration files.

$ PHP_INI_SCAN_DIR=/usr/local/etc/php.d: php
  PHP will load all files in /usr/local/etc/php.d/*.ini, then
  /etc/php.d/*.ini as configuration files.
```

## User.ini files

Since PHP 5.3.0, PHP includes support for configuration INI files on a per-directory basis. These files are processed only by the CGI/FastCGI SAPI.

In addition to the main php.ini file, PHP scans for INI files in each directory, starting with the directory of the requested PHP file, and working its way up to the current document root (as set in $_SERVER['DOCUMENT_ROOT']). In case the PHP file is outside the document root, only its directory is scanned.

Only INI settings with the modes PHP_INI_PERDIR and PHP_INI_USER will be recognized in .user.ini-style INI files.

## Where a configuration setting may be set

These modes determine when and where a PHP directive may or may not be set. For example, some settings may be set within a PHP script using ini_set(), whereas others may require php.ini or httpd.conf.


* PHP_INI_USER -	Entry can be set in user scripts (like with ini_set()) or in the Windows registry. Since PHP 5.3, entry can be set in .user.ini

* PHP_INI_PERDIR	- Entry can be set in php.ini, .htaccess, httpd.conf or .user.ini (since PHP 5.3)
* PHP_INI_SYSTEM	- Entry can be set in php.ini or httpd.conf
* PHP_INI_ALL	- Entry can be set anywhere

# Tips

* environment variables can be used in php.ini

```php
; PHP_MEMORY_LIMIT is taken from environment
memory_limit = ${PHP_MEMORY_LIMIT}
```

* Since PHP 5.1.0, it is possible to refer to existing .ini variables from within .ini files

```php
 open_basedir = ${open_basedir} ":/new/dir".
```

* Also you can use PHP constants in php.ini, but DONT'T wrap them in ${} or "".

```php
error_log = "/data/"PHP_VERSION"/"
```
