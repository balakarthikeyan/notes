## Require PHP Unit 
```
composer require --dev phpunit/phpunit ^9.2.5

echo @php "%~dp0phpunit.phar" %* > phpunit.cmd

phpunit --version
```

## PHP Unit <commands>
```
.   Printed when the test succeeds.
F   Printed when an assertion fails while running the test method.
E   Printed when an error occurs while running the test method.
R   Printed when the test has been marked as risky (see Chapter 6).
S   Printed when the test has been skipped (see Chapter 7).
I   Printed when the test is marked as being incomplete or not yet implemented (see Chapter 7).
W   Printed when the test has warnings.
```

## Run Locally:
```
php phpunit-9.2.5.phar tests\filename.php
php phpunit-9.2.5.phar phpunit --bootstrap ./vendor/autoload.php tests/filename.php --coverage-text="coverage.txt"
```

## Run Globally
```
phpunit tests/
phpunit tests/filename.php
phpunit tests/ --filter filename
```
