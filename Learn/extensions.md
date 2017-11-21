


# PHP extensions

PHP extensions are compiled libraries which enable specific functions to be used in your PHP code.

For example, you want to interact with MySQL using PHP. You can implement your own methods to connect to MySQL server, make queries using TCP/IP protocol. However thats not a trivial task. Plus, that is not only your own requirement, but other developers also need to do similar thing.

Therefore, someone already wrote a `plugin` and made it available. Such plugins can be static compiled (so 'bundled' with php package) or can be dynamically enabled by installing later on (on Windows in form of a DLL file, on other systems .so file for example).

There are two types of extensions you can install: bundled with PHP but not installed by default, and third party extensions.

Extensions can be loaded by adding an extension directive to the php.ini file.

##### Defaul folder to find php extensions

CentOS

```shell

/usr/lib64/php/modules/

```


# installing php extensions 


## Ubuntu 

### Using PEAR/PECL

First install pear 

```shell

sudo apt-get install php-pear

```

Then install php-devel tools to compile it

```shell

sudo apt-get install phpx.x-dev

```

The install the extension that you wants

```shell

#example of installing the zip extensions
sudo pecl install zip 

#it will generate the *.so file in '/usr/lib/php/<current_date>/zip.so

#find the extension_dir running
cat /path/to/php.ini | grep extension_dir

#copy the generated *.so file into the extension dir
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


### from PHP source

 When installing a bundled extension, you’ll need the source code of PHP on your machine.
 

```shell

#example of getting the php source, extracting the archive and going into that folder

wget http://be2.php.net/distributions/php-5.5.12.tar.bz2
tar xvjf php-5.5.12.tar.bz2
cd php-5.5.12
 
 ```
To see the sources of all bundled extensions, go into the ext folder inside the unarchived PHP source code folder.
Then go the folder of the extension and runs:

```shell

#To install extensions from source, we need the PHP dev tools installed on our machine, as well as a compiler that can produce #the extension file.

sudo apt-get install phpx-dev phpx-gcc libpcre3-dev

#example with the intl extension
cd intl


#phpize prepares the extension’s folder for compliation. It allows you to do the subsequent commands by creating a configure #file, and basically making the extension’s folder “think” it’s PHP itself. The procedure after phpize is, in fact, identical #to what you would do when installing PHP from source – only in this case, just a fragment of PHP is compiled and prepared #for use with the already compiled and installed PHP.
phpize

#./configure --enable-intl configures the environment for compilation. It prepares everything the compiler will need to craft #the intl.so file which we’ll be using. The enable-intl flag is necessary even though we’re in the intl folder because the #folder, effectively, thinks it is PHP, and we need to help it live that illusion. This command tells it: “Ok, you’re PHP’s #source code. Now compile and install with the intl extension.”, when in fact, it’s the only part that can be installed from #this folder.
./configure --enable-intl

#make will compile the sources into intl.so, placing the file into the very folder you’re currently in, under the modules #subfolder.
make

#sudo make install will move this file into the current PHP installation’s extensions folder.
sudo make install

#enable the extension in php.ini

```

### Compiling shared PECL extensions with phpize

If you need to build an extension, you can use the lower-level build tools to perform the build manually.

```shell

#To install extensions from source, we need the PHP dev tools installed on our machine, as well as a compiler that can produce #the extension file.

sudo apt-get install php-devel gcc

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

### Compiling PECL extensions statically into PHP

you'll need to place the extension source under the /your/phpsrcdir/ext/ directory and tell the PHP build system to regenerate its configure script.


```shell

#move to the php src dir
cd /your/phpsrcdir/ext

pecl download extname

gzip -d < extname.tgz | tar -xvf -

#This will result in the following directory: /your/phpsrcdir/ext/extname
mv extname-x.x.x extname 

#then force PHP to rebuild the configure script

rm configure

./buildconf --force

./configure --help

#Whether --enable-extname or --with-extname is used depends on the extension. Typically an extension that does not require external #libraries uses --enable
./configure --with-extname --enable-someotherext --with-foobar

#then  build PHP as normal

make

make install

#to be sure

./configure --help | grep extname

```
 Note: Some extensions cannot be statically linked (e.g., xdebug).

### using aptitude package manager


For example, to install the mcrypt extension for PHP 7.1 on ubuntu 16.04:

```shell

sudo apt-get install php7.1-mcrypt

```

This will put an mcrypt.ini file on the /etc/php/7.1/mods-available folder. Then you should symlink this file for the cli and for fpm

```shell

sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/cli/conf.d/20-mcrypt.ini
sudo ln -s /etc/php/7.1/mods-available/mcrypt.ini /etc/php/7.1/fpm/conf.d/20-mcrypt.ini

```

# Removing Extensions

to remove extensions, there’s **no need to delete any actual files** unless you’re really low on space. You can do it in three ways:

* Run php5dismod if you have the tool available. It’s the opposite of the php5enmod tool. The .so files will stay in place, and the ini files will remain in mods-available, they just won’t load because their symlinks will be removed from the fpm and cli conf.d folders.

* Remove the symlinks manually. E.g. sudo rm /etc/php5/cli/conf.d/mongo.ini

* If you enabled the extensions by putting them directly into the php.ini files, remove those lines from the php.ini files, or better yet, comment them so that they remain accessible for further use should you ever change your mind.


# wrtiting PHP extensions

I would recommend to read this articles and the comments: [PHP Extensions – How and Why?](http://abhinavsingh.com/php-extensions-how-and-why/)

# Resources

[How to Install PHP Extensions from Source](https://www.sitepoint.com/install-php-extensions-source/)
[PHP Extensions – How and Why?](http://abhinavsingh.com/php-extensions-how-and-why/)
