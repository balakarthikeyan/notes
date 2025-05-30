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

### Install PHP 8
```
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

### Install PHP as Apache
```
sudo apt update
sudo apt install php8.0 libapache2-mod-php8.0
sudo apt install php8.0-mysql php8.0-gd
sudo systemctl restart apache2
```

### Configure Apache with PHP-FPM
```
sudo apt update
sudo apt install php8.0-fpm libapache2-mod-fcgid
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php8.0-fpm
systemctl restart apache2
```

### Installing Nginx
```
sudo apt update
sudo apt install php8.0-fpm
systemctl status php8.0-fpm
sudo systemctl restart nginx
```

### Installing Cert
```
ssh-keygen -t rsa
ssh-copy-id -p 2222 vagrant@127.0.0.1
cat <username>/.ssh/id_rsa.pub | ssh vagrant@12.0.0.1 "mkdir <username>/.ssh/; cat >> <username>/.ssh/authorized_keys"
vagrant ssh-config > vagrant-ssh; ssh -F vagrant-ssh default
```


What is Webpack?

Webpack is a popular JavaScript module bundler that is used to build and compile complex web applications. It is a powerful tool that can transform, bundle, and optimize the assets of your application, including JavaScript, CSS, images, and more.

key features of webpack:

Module Bundling: Webpack takes your application’s source code, which may contain many different files, and combines them into a single file or set of files, known as a bundle. This makes it easier to manage your code and reduces the number of requests required to load your application.
Code Splitting: Webpack allows you to split your code into smaller chunks that can be loaded on demand. This can improve the performance of your application, as only the code that is required for a particular page or feature is loaded.
Loader System: Webpack has a flexible and extensible loader system that allows you to process different types of files, such as images, CSS, or even TypeScript. This makes it easy to integrate a variety of assets into your application.
Plugins: Webpack has a rich ecosystem of plugins that provide additional functionality and optimizations, such as code minification, tree shaking, and more.
Dev Server: Webpack comes with a built-in development server that makes it easy to get started with your application. The dev server provides hot reloading, which means that you can see the changes you make to your code in real-time, without having to manually refresh the page.

To get started with Webpack, you’ll need to install it using npm:

npm install webpack webpack-cli --save-dev

need to create a “webpack.config.js” file that will configure how Webpack should behave:

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist',
  },
};

Webpack-bundle-analyzer scans the bundle and builds a visualization of packages inside it

npm install —-save-dev webpack-bundle-analyzer
In our package.json we can add commands:

"profile": "webpack --config webpack.client.js --profile --json > stats.json"
"analyzer": "webpack-bundle-analyzer ./stats.json ./path/to/your/bundle -h 0.0.0.0"

Profiling needs a lot of memory and cpu, so we should add more resources for our docker ( Docker -> Preferences -> Resources — CPU min 4 Cores and Memory 2Gb )

Next we could run profiling:

docker-compose run your-service-image-name npm run profile
After finished profiling:

docker-compose run -p 8888:8888 your-service-image-name npm run analyzer
In your browser on localhost:8888