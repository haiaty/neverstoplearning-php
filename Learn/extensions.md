
Extensions can be loaded by adding an extension directive to the php.ini file.



### installing php extensions compiling it from source

```shell

#To install extensions from source, we need the PHP dev tools installed on our machine, as well as a compiler that can produce #the extension file.

sudo apt-get install phpx-dev php5-gcc libpcre3-dev

#Browse to the directory you'd like the "temp" files to be stored at, in this case /root:
cd /root

#Next we'll download and extract the actual PECL extension tar.gz file (be sure to replace the PECL extension with the one you #want):
wget http://pecl.php.net/get/uploadprogress-1.0.3.1.tgz && tar zxvf uploadprogress-1.0.3.1.tgz

#Next cd into the new directory and prepare for compiling:
 phpize && ./configure -with-php-config=/usr/bin/php-config

#Once finished we'll compile and install it:
make && make install

#then you shoudld copy the generated *.so file into the extension dir
#that you can find running
cat /path/to/php.ini | grep extension_dir

cp /path/to/.so/ /path/to/extension_dir/

#then you should enable the extension and add the configurations for it in the .ini file
# or changingng the php.ini file and adding
extension = module.so
add other extension configs

#or adding a .ini file in the folder mods-available and creating a soft link to cli and fpm
#see this EXAMPLE:

sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/cli/conf.d/20-mcrypt.ini
sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/fpm/conf.d/20-mcrypt.ini

```

### installing php extensions on unix using the OS package manager

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

# Resources

[How to Install PHP Extensions from Source](https://www.sitepoint.com/install-php-extensions-source/)
