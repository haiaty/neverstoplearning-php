

# PEAR

PEAR is PHP Extension and Application Repository, it has libraries and code written IN php. Those you can simply download, install and include in your code.

It's a repository of libraries, but it's also a distribution channel/packaging system. Originally, the packaging system just distributed the single PEAR repository, but today, the distribution channel can be used by any third party library. 

Each PEAR code package comprises an independent project under the PEAR umbrella. It has its own development team, versioning-control and documentation.

**PEAR packages**

The smallest unit that can be managed by Pyrus or the PEAR Installer is a package. A package is a collection of files that are organized and defined by a meta-information file called package.xml.
A package also contains meta-information about the collected files, such as the name of the package, the channel that the package is from, the version of the package, information on the developers who created the package, and any external dependencies the package has on other packages or installation requirements (such as minimum PHP version).

Packages can exist as a collection of files on disk, or can be placed into an archive in phar, tar, or zip format and then later installed on another system.

PEAR coding standards require packages to be renamed when they break backwards compatibility. Thus, a PEAR package **can never reach version 2.0.0**.

**PEAR channels**

A PEAR Channel is a web site that distributes package archives for remote installation by users of Pyrus or the PEAR Installer. 

#### PEAR package manager

The PEAR package manager provides a standardized way to install, uninstall, or upgrade with new PEAR packages or PECL extensions. Before installing a package it can also be instructed to take care of package dependencies so all the extra needed packages are installed too.

The PEAR package manager is run from the command line using the pear command. 

#### PEAR and PECL

PECL is conceptually very similar to PEAR, and indeed PECL modules are installed with the PEAR Package Manager. 


#### PEAR and Composer

With Composer there is an alternative available for managing packages for a PHP project. Composer also supports the installation of PEAR packages


# Resources

[ PEAR manual](http://pear.php.net/manual/en/)
[ PEAR from wikipedia](https://en.wikipedia.org/wiki/PEAR) 
