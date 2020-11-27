## To setup XAMPP

Download Vagrant and VirtualBox, Once Vagrant and VirtualBox are installed in Command prompt use the below commands to setup

```
vagrant version
vagrant init <box>
vagrant up
```
## Basic Commands
    Run `vagrant ssh` to connect to the VM
    Run `vagrant status` to check what state is the VM
    Run `vagrant reload` to restart the VM which runs `halt` and `up` the box
    Run `vagrant halt` to stop the VM
    Run `vagrant destroy` to wipe the VM

### Frequently used Ubuntu Boxes
To search more box https://app.vagrantup.com/boxes/search
```
vagrant init precise32          //Ubuntu 12.04.3 LTS
vagrant init ubuntu/trusty64    //Ubuntu 14.0
vagrant init ubuntu/xenial64    //Ubuntu 16.04
vagrant init bento/ubuntu-18.04 //Ubuntu 18.04
```

### To check Ubuntu version
`lsb_release -a`

### To install Apache
`sudo apt-get install apache2` to test the installation `apache2 -v`

### To configure server name
open `sudo vi /etc/apache2/apache2.conf` add `ServerName localhost`
to add new domain for private network `sudo vi /etc/hosts` 

### Restart Server
`sudo service apache2 restart`

### Test Configuration settings
`sudo apache2ctl configtest`

### To install PHP
```
sudo apt update
sudo apt upgrade
````

### To install PHP 7.x using PPA (Personal Package Archive) to install this non-standard software.
```
sudo apt-add-repository ppa:ondrej/php
sudo apt-get update
```

### To install PHP
`sudo apt-get install -y <php>` to test installation check `php -v`

### Default Extensions:
```
sudo apt-get install php7.3-cli
sudo apt-get install php7.3-json
sudo apt-get install php7.3-pdo
sudo apt-get install php7.3-mysql
sudo apt-get install php7.3-curl
sudo apt-get install php7.3-gd
sudo apt-get install php7.3-bcmath
sudo apt-get install php7.3-cgi
sudo apt-get install php7.3-ldap
sudo apt-get install php7.3-mbstring
sudo apt-get install php7.3-xml
sudo apt-get install php7.3-soap
sudo apt-get install php7.3-xsl
sudo apt-get install php7.3-zip
```

### To confirm extension
`sudo apt policy <package>`

### To install MySQL
```
sudo apt-get install mysql-server
mysql -u root -p
```

### To verify installation
`dpkg -l | grep "apache2\|mysql-server\|php"`

### Create custom box package and load

```
vagrant package --output ubuntu_LAMP.box
mkdir boxes
move ubuntu_LAMP.box boxes
vagrant init ubuntu_LAMP file:boxes/ubuntu_LAMP.box
```

### Provisioning with script.sh
`config.vm.provision :shell, path: "script.sh"`

### Common commands for Vagrant
```
vagrant box list
vagrant box remove -f
vagrant ssh-config
```

## Troubleshot

### To remove repository & source list
```
sudo apt-add-repository --remove ppa:ondrej/php
sudo ls | sudo rm -i /etc/apt/sources.list.d/
```

### Add below in source list if any issues in ppa
```
sudo vi /etc/apt/sources.list
`for Ubuntu 18.04`
deb http://ppa.launchpad.net/ondrej/php/ubuntu bionic main
deb-src http://ppa.launchpad.net/ondrej/php/ubuntu bionic main
`for Ubuntu 16.04`
deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main
deb-src http://ppa.launchpad.net/ondrej/php/ubuntu xenial main
```

### Check packages of PHP 7 & remove

```
dpkg --list | grep -i 'php7.*'
sudo apt-get --purge remove  php7.*
```

### Install PHP 7 Modules & check the packages

```
sudo apt-cache search php7.2
sudo apt-cache pkgnames | grep php7.2
```

### Change cli version back to 7.0

```
sudo update-alternatives --set php /usr/bin/php7.0
sudo update-alternatives --set phar /usr/bin/phar7.0
sudo update-alternatives --set phar.phar /usr/bin/phar.phar7.0
sudo update-alternatives --set phpize /usr/bin/phpize7.0
sudo update-alternatives --set php-config /usr/bin/php-config7.0
```

### Switch to php5.6 to php7.0

`sudo a2dismod php5.6 && sudo a2enmod php7.0 && sudo service apache2 restart`

## Common Configuration

### Apache

```
sudo nano /etc/php/7.2/apache2/php.ini
max_execution_time = 180
max_input_time = 360
max_input_vars = 5000
memory_limit = 256M
file_uploads = On
upload_max_filesize = 100M
allow_url_fopen = On
date.timezone = Asia/Calcutta
```

### PHP
> sudo nano /var/www/html/phpinfo.php
```
<?php phpinfo(); ?>
```

## Ubuntu Commands
    `ll`	to list the files
    `tree <foldername>`	to list files in tree structure
    `apt-get`	is command line software for installing packages on Ubuntu
    `history | tail 10`	to show only 10 history log
    `history -c` clear history
    `echo "$HISTFILE"`	to echo path of history
    `rm ~/.bash_history`	remove history file
    `echo 'history -c' >> ~/.bash_logout`	to delete history while logout
    `dpkg -l | grep php | tee packages.txt`	check the content and write in packages.txt
    `ls`	Show directory contents (list the names of files).
    `cd`	Change Directory.
    `mkdir`	Create a new folder (directory).
    `touch`	Create a new file.
    `rm`	Remove a file.
    `cat`	Show contents of a file.
    `pwd`	Show current directory (full path to where you are right now).
    `cp`	Copy file/folder.
    `mv`	Move file/folder.
    `grep`	Search for a specific phrase in file/lines.
    `find`	Search files and directories.
    `vi/nano`	Text editors.
    `history`	Show last 50 used commands.
    `clear`	Clear the terminal screen.
    `tar`	Create & Unpack compressed archives.
    `wget`	Download files from the internet.
    `du`	Get file size.
