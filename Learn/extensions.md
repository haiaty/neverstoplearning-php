
Extensions can be loaded by adding an extension directive to the php.ini file.

### install php extensions on unix

To install an extension you can use the package manager of the os that you are using.


### Mbcrypt


For example, to install the mcrypt extension for PHP 7.1 on ubuntu 16.04:

```shell

sudo apt-get install php7.1-mcrypt

```

This will put an mcrypt.ini file on the /etc/php/7.1/mods-available folder. Then you should symlink this file for the cli and for fpm

```shell

sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/cli/conf.d/20-mcrypt.ini
sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/fpm/conf.d/20-mcrypt.ini

```
