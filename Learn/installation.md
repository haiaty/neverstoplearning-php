

* [knowledge aggregation](#installation)
  * On unix
      * [Ubuntu 14 - 16](#ubuntu-1404---1610)
        * [Only PHP 7.1](#php-71---ubuntu)
        * [Install and configure Apache 2 and PHP-FPM on Ubuntu 14](#install-and-configure-apache-2-and-php-fpm-on-ubuntu-14)
      * [Centos / RHEL 6.8](#centos--rhel-68)
        * [PHP 7.1](#php-71---centos)
        * [PHP 5.3](#php-53---centos)
* [Resources](#resources)

# Installation

Before installing PHP, first you should have clear for what you will use it. will you use it for web development? will you use it for cli programs or for desktop applications?

Then, when you have clear that, you need to know [how to run](https://github.com/haiaty/neverstoplearning-php/blob/master/Learn/how-to-run.md) it in order to achieve the purpose for what you will be using it. 

If you choose if for web development, then you should choose wich web server you will use and how it will connect to php. would it be trough a module, as fastcgi or with php-fpm?

## On Unix

There are several ways to install PHP for the Unix platform, either with a compile and configure process, or through various pre-packaged methods.Many Unix like systems have some sort of package installation system. This can assist in setting up a standard configuration, but if you need to have a different set of features you may need to build PHP.

The initial PHP setup and configuration process is controlled by the use of the command line options of the configure script. You could get a list of all available options along with short explanations running ./configure --help. This is where you customize your PHP
with various options, like which extensions will be enabled. If you decide to change your configure options after installation, you only need reconfigure and rerun 'make' and 'make install'

When PHP is configured, you are ready to build the module and/or executables. The command make should take care of this.

Note that unless told otherwise, 'make install' will also install PEAR, various PHP tools such as phpize, install the PHP CLI, and more.


##### Ubuntu 14.04 - 16.10


##### note on multiple installations

If you have multiple php installations, you will find that config files are all in /etc/php/<version> (for example /etc/php/5.6 and /etc/php/7.0)

To check which one is used or change it 

```shell

php -v

#or 

ls -l  /etc/alternatives/ | grep php

```

###### PHP 7.1 - ubuntu

PHP 7.1 can be installed using Ondřej Surý's PPA:

```shell
sudo add-apt-repository ppa:ondrej/php
#If you get a command not found error for add-apt-repository, you can install it from:
#sudo apt-get install software-properties-common python-software-properties

sudo apt-get update
#this will create the folder /etc/php/7.1 with inside three folders apache2/, cli/, mods-available
sudo apt-get install php7.1 

#if you need to install php-fpm do
sudo apt-get install php7.1-fpm

```

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

---

# Installing on CentOS / RHEL 6.8

## PHP 7.1 - centos

First, you'll want to ensure that the EPEL repository is configured (and enable the optional channel for RHEL too)

### For centOS 7
```shell
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm

# For RHEL, run this command as well:
subscription-manager repos --enable=rhel-7-server-optional-rpms
```

Next we enable the remi-php71 repository:

```shell
yum install yum-utils
yum-config-manager --enable remi-php71

```

and install php 7.1

```shell
yum install php71
```

### For centOS 6.5

```shell
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

yum search php

yum install php71w

```


## PHP 5.3 - centos

```shell

yum update

#Note: if you don't find nothing you should add the repository
yum search php

#this will create:
# /usr/bin/php - excecutable
# /usr/bin/php-cgi
# /etc/php.d  where there will be ini files for extensions configurations
# /etc/php.ini the php ini configuration file

yum install php

```

# Uninstalling on centoOS

```shell

#first find the installed packages
rpm -qa | grep php

yum remove <packagename>

#example
yum remove php-common



```


# Resources

* [Installing PHP 7.1](https://www.colinodell.com/blog/2016-12/installing-php-7-1)
* [Installing Apache + MOD_FASTCGI + PHP-FPM on ubuntu server](https://alexcabal.com/installing-apache-mod_fastcgi-php-fpm-on-ubuntu-server-maverick/)


