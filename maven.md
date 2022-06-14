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

## How to clean old dependencies from maven repositories? 
I have too many files in .m2 folder where maven stores downloaded dependencies. Is there a way to clean all old dependencies? For example, if there is a dependency with 3 different versions: 1, 2 and 3, after cleaning there must be only 3rd. How I can do it for all dependencies in .m2 folder?  

If you are on Unix, you could use the access time of the files in there. Just enable access time for your filesystem, then run a clean build of all your projects you would like to keep dependencies for and then do something like this (UNTESTED!):
```
find ~/.m2 -amin +5 -iname '*.pom' | while read pom; do parent=`dirname "$pom"`; rm -Rf "$parent"; done
```
This will find all *.pom files which have last been accessed more than 5 minutes ago (assuming you started your builds max 5 minutes ago) and delete their directories.

Add "echo " before the rm to do a 'dry-run'.