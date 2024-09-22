### Install apache
```
sudo apt-get update
sudo apt-get install apache2
hostname -I | awk '{print $1}'
```

###  Configure UFW firewall to allow traffic on port 80.
```
sudo ufw show app list
sudo ufw allow 'Apache'
sudo ufw status
sudo ufw allow 'OpenSSH'
```

### Stop Apache:
```sudo systemctl stop apache2.service```

### Start Apache:
```sudo systemctl start apache2.service```

### Restart Apache:
```sudo systemctl restart apache2.service```

### Reload Apache:
```sudo systemctl reload apache2.service```

### Files
**apache2.conf**
The main Apache2 configuration file. Contains settings that are global to Apache2.

**envvars**
File where Apache2 environment variables are set.

**magic**
Instructions for determining MIME type based on the first few bytes of a file.

**ports.conf**
Houses the directives that determine which TCP ports Apache2 is listening on.

### Configuration: 
Apache runs with the user `nobody` and `httpd` daemon. Apache main configuration file are all located in the below path: 
`/etc/httpd/conf/httpd.conf` (CentOS/RHEL/Fedora) or `/etc/httpd/httpd.conf`
`/etc/apache2/apache2.conf` (Ubuntu/Debian) or `/etc/apache2/httpd.conf`

### Directories

**/etc/apache2/apache2.conf** – This is the main Apache configuration file and controls everything Apache does on your system. Changes here affect all the websites hosted on this machine.

**/etc/apache2/ports.conf** – The port configuration file. You can customize the ports Apache monitors using this file. By default, Port 80 is configured for http traffic.

**/etc/apache2/conf-available**
This directory contains available configuration files. All files that were previously in `/etc/apache2/conf.d` should be moved to `/etc/apache2/conf-available`.

**/etc/apache2/conf-enabled**
Holds symlinks to the files in `/etc/apache2/conf-available`. 

**/etc/apache2/mods-available**
This directory contains configuration files to both load modules and configure them. 

**/etc/apache2/mods-enabled**
Holds symlinks to the files in `/etc/apache2/mods-available` directory. 

**/etc/apache2/sites-available**
This directory has configuration files for Apache2 Virtual Hosts. A virtual host is a record of one of the websites hosted on the server.
Virtual Hosts allow Apache2 to be configured for multiple sites that have separate configurations.

**/etc/apache2/sites-enabled**
Holds symlinks to the files in `/etc/apache2/sites-available` directory. This directory holds websites that are ready to serve clients. The `a2ensite` command is used on a virtual host file in the `sites-available` directory to add sites to this location.

Whenever a configuration file is symlinked, it will be enabled the next time after Apache2 is restarted.

### Installing and handling modules
```sudo apt install mod-_package_name_```
The installation will enable the module automatically.
we can disable it with a2dismod:
```sudo a2dismod _package_name_```
And then use the a2enmod utility to re-enable it:
```sudo a2enmod _package_name_```
sudo systemctl restart apache2.service

### Sharing write permission
sudo chown -R $USER:$USER /var/www/html
sudo chgrp -R webmasters /var/www/html
sudo chmod -R g=rwx /var/www/html

### Setting Up Virtual Hosts 

Website content is stored in the `/var/www/html/directory`. You can create subdirectories within this location for each different website hosted on your server.

Instead of modifying the default configuration file located at ```/etc/apache2/sites-available/000-default.conf``` directly, 
let’s make a new one at ```/etc/apache2/sites-available/your_domain.conf```

```
/etc/apache2/sites-available/your_domain.conf
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

To automatically create a symbolic link in the `sites-enabled` directory to an existing file in the `sites-available` directory.
```sudo a2ensite your_domain```

After enabling a site, issue the following command to tell Apache to re-read its configuration files, allowing the change to propagate:
```sudo service apache2 reload```

There is also a companion command for disabling a Virtual Host. It operates by removing the symbolic link from the `sites-enabled` directory:
```sudo a2dissite your_domain```

Apache creates log files for any errors it generates in the file` /var/log/apache2/error.log`.

It also creates access logs for its interactions with clients in the file `/var/log/apache2/access.log`.

### Test for configuration errors:
```sudo apache2ctl configtest```