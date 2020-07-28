# Create Maven Project
mvn archetype:generate -DgroupId=com.java.samples -DartifactId=JavaSamples -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

# Description
- groupId will be com.java.samples, representing the package.
- artifactId will be JavaSamples (on the build, JavaSamples.jar will be created)
- archetypeArtifactId is nothing but the template used for creating this Java project. In the above case we defined maven-archetype-quickstart
- interactiveMode is used when the developer is aware of the actual spelling of the artifact id. If interactiveMode is set to be TRUE so that it will scan the remote repositories for all available archetypes.

# Environmental variables
Variable name : JAVA_HOME
Variable Value : C:\Program Files\Java\jdk1.8.0_202
Variable name : M2_HOME
Variable Value : C:\Program Files\apache-maven-3.6.0

# Version
mvn -v

# To download complete dependencies
mvn dependency:tree

# To clean and download dependencies
mvn dependency:purge-local-repository

# Clean Project
mvn clean

# Clean and download Project
mvn -U clean install

# Install
mvn install

# Complie
mvn compile

# Bundle
mvn package

# Validate
mvn validate -e

# Test
mvn test

# Deploy
mvn deploy

# Jetty Server Run
mvn jetty:run

# Run tomcat plugin
mvn tomcat7:deploy 
mvn tomcat7:undeploy 
mvn tomcat7:redeploy 

# Creating a JAR File
jar cf jar-file.jar A.class B.class C.class

# Viewing a JAR File
jar tf jar-file.jar

# Running a JAR File
java -jar jar-file.jar
java -cp target/jar-file.jar com.package.HelloWorld //if packageName not provided className can given