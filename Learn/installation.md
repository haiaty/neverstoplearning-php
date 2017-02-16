

* [knowledge aggregation](#installation)
  * [On unix]
      * [Ubuntu 14 - 16](#ubuntu-1404---1610)
        * [PHP 7.1](#php-71-ubuntu)
      * [Centos / RHEL 6.8](#centos--rhel-68)
        * [PHP 7.1](#php-71-centos)
* [Resources](#resources)

# Installation

## On Unix

There are several ways to install PHP for the Unix platform, either with a compile and configure process, or through various pre-packaged methods.Many Unix like systems have some sort of package installation system. This can assist in setting up a standard configuration, but if you need to have a different set of features you may need to build PHP.

The initial PHP setup and configuration process is controlled by the use of the command line options of the configure script. You could get a list of all available options along with short explanations running ./configure --help. This is where you customize your PHP
with various options, like which extensions will be enabled. If you decide to change your configure options after installation, you only need reconfigure and rerun 'make' and 'make install'

When PHP is configured, you are ready to build the module and/or executables. The command make should take care of this.

Note that unless told otherwise, 'make install' will also install PEAR, various PHP tools such as phpize, install the PHP CLI, and more.

##### Ubuntu 14.04 - 16.10

###### PHP 7.1 - ubuntu

PHP 7.1 can be installed using Ondřej Surý's PPA:

```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.1

```

---

##### CentOS / RHEL 6.8

###### PHP 7.1 - centos

First, you'll want to ensure that the EPEL repository is configured (and enable the optional channel for RHEL too)

```
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm

# For RHEL, run this command as well:
subscription-manager repos --enable=rhel-7-server-optional-rpms

```
Next we enable the remi-php71 repository:

```
yum install yum-utils
yum-config-manager --enable remi-php71

```

and install php 7.1

```
yum install php71

```


# Resources

[Installing PHP 7.1](https://www.colinodell.com/blog/2016-12/installing-php-7-1)


