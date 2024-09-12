# Install Oracle JDK 6 on Linux

## Download
[JDK 6](https://www.oracle.com/java/technologies/javase-java-archive-javase6-downloads.html)

## Instalar

- Give it permissions to execute and extract it  
```
chmod a+x [version]-linux-i586.bin
```

- Moverse a la carpeta y ejecutar el siguiente comando
```
./[version]-linux-i586.bin
``` 

- Copiar la carpeta en la ubicacion /opt/jdk/

- Crear un enlace simbolico
```
sudo ln -s /opt/jdk/jdk1.6.0_45/ /opt/jdk1.6
```