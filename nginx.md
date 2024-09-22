### Install NGINX
```
sudo apt update
sudo apt install nginx
```

### Adjusting the Firewall
```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```

### Checking your Web Server
```systemctl status nginx```

### Managing the Nginx Process
```
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx
```

### Setting Up Server Blocks
```
sudo mkdir -p /var/www/your_domain/html
sudo chown -R $USER:$USER /var/www/your_domain/html
sudo chmod -R 755 /var/www/your_domain
nano /var/www/your_domain/html/index.html
```

Add the following configuration block, which is similar to the default, but updated for your new directory and domain name:
```
/etc/nginx/sites-available/your_domain
server {
        listen 80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain.com www.your_domain;

        location / {
            try_files $uri $uri/ =404;
        }
}
```

### Check syntax errors in any of your Nginx files:
`sudo nginx -t`

### Nginx Files and Directories
`/var/www/html:` The actual web content

### Server Configuration
- `/etc/nginx:` The Nginx configuration directory. 
- `/etc/nginx/nginx.conf:` The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.
- `/etc/nginx/sites-available/:` Nginx will not use the configuration files found in this directory unless they are linked to the sites-enabled directory.
- `/etc/nginx/sites-enabled/:` These are created by linking to configuration files found in the sites-available directory.
- `/etc/nginx/snippets:` This directory contains configuration fragments that can be included elsewhere in the Nginx configuration.

### Server Logs
- `/var/log/nginx/access.log:` Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.
- `/var/log/nginx/error.log:` Any Nginx errors will be recorded in this log.