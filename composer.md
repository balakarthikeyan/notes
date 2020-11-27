## Usage of Composer via PHP
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); }"
php composer-setup.php
php -r "unlink('composer-setup.php');"
``` 

## Run Locally:
```
echo @php "%~dp0composer.phar" %*>composer.bat
php composer.phar start
php composer.phar install
php composer.phar update
php composer.phar dump-autoload
php -S localhost:8080 -t public //public is the folder to run
```

## Run Globally:
```
composer start
composer -v
composer init
composer install
composer update
composer update --no-scripts
composer require slim/slim "^3.0"
composer require balakarthikeyan/hello-composer "dev-master"
composer remove vendor_name/package_name
composer show
```
