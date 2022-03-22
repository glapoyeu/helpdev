## Deploy on tomcat without tests on path project
mvn tomcat7:redeploy -Dmaven.test.skip=true

## Deploy on tomcat without tests on path project and clean install target
mvn clean install tomcat7:redeploy -Dmaven.test.skip=true


## Package
mvn package

## Run a jar
 java -jar project.jar


# How to properly remove a dependency in a Maven project
mvn dependency:purge-local-repository  
or  
mvn dependency:purge-local-repository -DreResolve=false

