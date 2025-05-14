## Usage of Composer via PHP
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); }"
php composer-setup.php
php -r "unlink('composer-setup.php');"
``` 
## Re Installation
```bash
sudo rm /usr/bin/composer
which composer
curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/bin/composer
```

## Run Locally:
```bash
echo @php "%~dp0composer.phar" %*>composer.bat
php composer.phar start
php composer.phar install
php composer.phar update
php composer.phar dump-autoload
php -S localhost:8080 -t public //public is the folder to run
```

## Run Globally:
```bash
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

## My package
```json
{
  "repositories": [
    {
      "type": "vcs",
      "url": "https://github.com/balakarthikeyan/hello-composer"
    }
  ],
  "require": {
    "monolog/monolog": "master",
    "balakarthikeyan/hello-composer": "dev-master"
  },
  "minimum-stability": "stable"
}

Packgist Token: 5d0e18d2c6cac315a19143cbb214b40a0b4da965
```

## Composer Autoload
```bash
composer dump autoload -o
composer update --no-scripts
composer update --with-all-dependencies
composer update --dry-run
composer install --no-dev --optimize-autoloader
composer install --no-ansi --no-dev --no-interation --no-progress --no-scripts --optimize-autoloader
```
- Skip Dev dependencies `( --no-dev )` 
- Optimize autoloader `( --optimize-autoload )` 
- Stop installation scripts (might cause issue during build but completely dependent on your business logic)`( --no-scripts )` 
- It will not print progress and will work without generating o/p or asking for any i/p `( --no-progress --no-interaction and --no-ansi )`