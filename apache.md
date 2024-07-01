Install apache
sudo apt-get update
sudo apt-get install apache2
hostname -I | awk '{print $1}'

Configure Your Firewall , UFW firewall to allow traffic on port 80.
sudo ufw show app list
sudo ufw allow 'Apache'
sudo ufw status
sudo ufw allow 'OpenSSH'

Stop Apache:
sudo systemctl stop apache2.service

Start Apache:
sudo systemctl start apache2.service

Restart Apache:
sudo systemctl restart apache2.service

Reload Apache:
sudo systemctl reload apache2.service

To automatically create a symbolic link in the "sites-enabled" directory to an existing file in the "sites-available" directory, issue the following command:
sudo a2ensite virtual_host_file_name

After enabling a site, issue the following command to tell Apache to re-read its configuration files, allowing the change to propagate:
sudo service apache2 reload

There is also a companion command for disabling a Virtual Host. It operates by removing the symbolic link from the "sites-enabled" directory:
sudo a2dissite virtual_host_file_name

Website content is stored in the/var/www/html/directory. 
You can create subdirectories within this location for each different website hosted on your server.

Apache creates log files for any errors it generates in the file /var/log/apache2/error.log.

It also creates access logs for its interactions with clients in the file /var/log/apache2/access.log.

Like many Linux-based applications, Apache functions through the use of configuration files. They are all located in the /etc/apache2/ directory.

Here’s a list of other essential directories:

/etc/apache2/apache2.conf – This is the main Apache configuration file and controls everything Apache does on your system. Changes here affect all the websites hosted on this machine.
/etc/apache2/ports.conf – The port configuration file. You can customize the ports Apache monitors using this file. By default, Port 80 is configured for http traffic.
/etc/apache2/sites-available – Storage for Apache virtual host files. A virtual host is a record of one of the websites hosted on the server.
/etc/apache2/sites-enabled – This directory holds websites that are ready to serve clients. The a2ensite command is used on a virtual host file in the sites-available directory to add sites to this location.