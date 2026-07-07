## On Windows, execute:
```
C:\sonarqube\bin\windows-x86-xx\StartSonar.bat
Log in to http://localhost:9000 with System Administrator credentials (login=admin, password=admin)
```
## Environment variable:
`SONARQUBE_HOME = C:\sonarqube\bin`

## Token:
`example: 6fa0799f50fe0c2ef066d83036b5dd161bf6f567`

## To Scan PHP
`sonar-scanner.bat -Dsonar.projectKey=6fa0799f50fe0c2ef066d83036b5dd161bf6f567 -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=6fa0799f50fe0c2ef066d83036b5dd161bf6f567`

## To scan Java
`mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login =6fa0799f50fe0c2ef066d83036b5dd161bf6f567`
  
## To change sonar.properties:

> For Mysql
```conf
sonar.jdbc.username=root
sonar.jdbc.password=
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
```
> For Java
```conf
sonar.web.javaOpts=-server
sonar.web.host=10.236.129.167
sonar.web.context=/sonarqube-dev
sonar.web.port=8081  
sonar.path.logs=logs
sonar.path.data=data
sonar.path.temp=temp
```
## To change JVM C:\sonarqube\conf\wrapper.conf
`wrapper.java.command=C:\Program Files\Java\jre1.8.0_201\bin\java`

## Configure SonarQube as a Windows service:

> Install/uninstall NT service
```bash
%SONARQUBE_HOME%/bin/windows-x86-32/InstallNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-32/UninstallNTService.bat
```
> Start/stop the service
```bash
%SONARQUBE_HOME%/bin/windows-x86-32/StartNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-32/StopNTService.bat
```
## To Scan PHP via Docker
```bash
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube #start the SonarQube server

# Used for installing in mac
brew update
brew install --cask java
brew install sonar-scanner

#sonar-project.properties file in the root of the project (public_html) that looks something like this:

sonar.projectKey=project-key
sonar.projectName=Project Name
sonar.projectVersion=1.0.0
sonar.login=4CqVnlU9PzqKUjBqaHu3oGiPhHAv1PrC4pRrhgOA
sonar.sources=wp-content/themes/,wp-content/plugins/

docker start sonarqube #start
docker stop sonarqube #stop

sonar-scanner -X > ~/Desktop/sonar-log.txt 2>&1 &
```

You can help us to provide a default Quality Profile covering WordPress's standards even without thinking about writing custom rules.

## What you need to do is to study the 164 rules available in SonarPHP and tell us:

- if the rule is relevant for WordPress
- if this is the case, which part of WordPress standards the rule is covering
- if the rule is no useable out of box for WordPress, tell us why
- if you believe some rules are missing to fully cover WordPress's standards, tell it and we will do our best to close the gap.

The best way to share this information with us would be a Google Sheet but whatever format that suits you will work for us. The identifier of a rule at SonarSource is looking like that: RSPEC-4426

## The WordPress Coding Standards package requires:

PHP 7.2 or higher with the following extensions enabled:
- Filter
- libxml
- Tokenizer
- XMLReader
- Composer
- iconv
- Multibyte String

## References

- https://github.com/WordPress/WordPress-Coding-Standards
- https://github.com/WordPress/WordPress-Coding-Standards/wiki/Running-in-GitHub-Actions
- https://github.com/WordPress/WordPress-Coding-Standards/wiki/Customizable-sniff-properties
- https://github.com/PHPCompatibility/PHPCompatibility
- https://github.com/sirbrillig/phpcs-variable-analysis/
- https://github.com/Automattic/VIP-Coding-Standards
