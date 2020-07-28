## On Windows, execute:
C:\sonarqube\bin\windows-x86-xx\StartSonar.bat
Log in to http://localhost:9000 with System Administrator credentials (login=admin, password=admin)

## Environment variable:
SONARQUBE_HOME = C:\ACE\sonarqube\bin

## Token:
example: 6fa0799f50fe0c2ef066d83036b5dd161bf6f567

## To Scan PHP
sonar-scanner.bat -Dsonar.projectKey=6fa0799f50fe0c2ef066d83036b5dd161bf6f567 -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=6fa0799f50fe0c2ef066d83036b5dd161bf6f567

## To scan Java
mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login =6fa0799f50fe0c2ef066d83036b5dd161bf6f567
  
## To change sonar.properties:

## For Mysql
sonar.jdbc.username=root
sonar.jdbc.password=
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false

## For Java
sonar.web.javaOpts=-server
sonar.web.host=10.236.129.167
sonar.web.context=/sonarqube-dev
sonar.web.port=8081  
sonar.path.logs=logs
sonar.path.data=data
sonar.path.temp=temp

# To change JVM C:\sonarqube\conf\wrapper.conf
wrapper.java.command=C:\Program Files\Java\jre1.8.0_201\bin\java

## Configure SonarQube as a Windows service:

## Install/uninstall NT service
%SONARQUBE_HOME%/bin/windows-x86-32/InstallNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-32/UninstallNTService.bat

## Start/stop the service
%SONARQUBE_HOME%/bin/windows-x86-32/StartNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-32/StopNTService.bat