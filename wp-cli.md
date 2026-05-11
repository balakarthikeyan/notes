# What Is WP-CLI? 
WP-CLI is the WordPress Command Line Interface, and you can use it to install a WordPress site, add new plugins or themes, create and modify users… and so much more.

### Download
```bash
$ wp core download --path=DIRECTORY_NAME
$ export PATH=$PATH:/Applications/MAMP/Library/bin/
$ wp config create --dbname=DATABASE_NAME --dbuser=root --dbpass=root
$ wp config create --dbname=demo --dbuser=root --dbpass=root --dbhost=127.0.0.1
$ wp db create
$ wp core install --url=localhost/demo --title="Demo Title" --admin_user=admin --admin_password=password --admin_email=test@example.com
```

## Top 5 WP-CLI Commands
Here are five basic commands you can try today to get started with WP-CLI.

### Install and Activate Plugins

`wp plugin install pantheon-advanced-page-cache --activate`

This one-line command will install and activate the `Pantheon Advanced Page Cache plugin`.

### Deactivate Plugins

`wp plugin deactivate plugin-name`

You can also deactivate a plugin—a great way to recover a site when a plugin or theme has brought your dashboard down. Just disable the plugin using WP-CLI, and you're back in business. 

### Change User Passwords

`wp user update 1 --user_pass=<password>`

You can also create new users, modify user roles, and more.

### Update Plugins

`wp plugin list --update=available`

This command will show you a list of all plugins on your site—and with the addition of the `--update=available` flag, you'll get only the plugins waiting to be updated. 

### Search and Replace

```bash
wp search-replace https://test.example.com https://live.example.com --allow-root
wp search-replace test.example.com live.example.com --allow-root
wp search-replace https%3A%2F%2Ftest.example.com https%3A%2F%2Flive.example.com --allow-root
wp search-replace https:\/\/test.example.com https:\/\/live.example.com --allow-root
wp search-replace https://test.example.com https://live.example.com --all-tables --verbose
```