

* [knowledge aggregation](#installation)


# Installation

## On Unix

There are several ways to install PHP for the Unix platform, either with a compile and configure process, or through various pre-packaged methods.Many Unix like systems have some sort of package installation system. This can assist in setting up a standard configuration, but if you need to have a different set of features you may need to build PHP.

The initial PHP setup and configuration process is controlled by the use of the command line options of the configure script. You could get a list of all available options along with short explanations running ./configure --help. This is where you customize your PHP
with various options, like which extensions will be enabled. If you decide to change your configure options after installation, you only need reconfigure and rerun 'make' and 'make install'

When PHP is configured, you are ready to build the module and/or executables. The command make should take care of this.

Note that unless told otherwise, 'make install' will also install PEAR, various PHP tools such as phpize, install the PHP CLI, and more.
