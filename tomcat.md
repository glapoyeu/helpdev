## Agregar el bin y el catalina_home en el path
export CATALINA_HOME=/home/tomcat // sino  se agrega en el .bashrc pone por defecto el directorio de donde se ha levantado el tomcat
## Arrancar tomcat
Antes se debe arregar en el fichero bashrc le path de java: export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.../
cd /home/tomcat/bin
./startup.sh // lanza un proceso java que es el propio tomcat, una instancia java (JVM)

## prueba
 localhost:8080 //Puerto por defecto

## Agregar funcionalidad de deploy en tomcat en pom.xml en maven correr mvn tomcat7:redeploy
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <url>http://localhost:18080/manager/text</url>
        <server>TomcatServer</server>
        <path>/televida-chat-ws-api</path>
        <warFile>${project.build.directory}/${project.build.finalName}-local.war</warFile>
    </configuration>
</plugin>