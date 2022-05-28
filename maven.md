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

## Instalar una dependencia manualmente
```
mvn dependency:purge-local-repository -DmanualInclude="my.group.id" -DsnapshotsOnly=true  -DactTransitively=false -DreResolve=false
```
```
mvn install:install-file -Dfile=mimepull-1.9.3.jar -DgroupId=org.jvnet.mimepull -DartifactId=mimepull -Dversion=1.9.3 -Dpackaging=jar
```

