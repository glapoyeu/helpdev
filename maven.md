## Deploy on tomcat without tests on path project
mvn tomcat7:redeploy -Dmaven.test.skip=true

## Package
mvn package

## Run a jar
 java -jar project.jar

