# Comands

## Descomprimir fichero con tar
```
tar xvf Download/apache-tomcat-9.0.36.tar.gz
```

## Rename name file descomprimido
```
mv apache-tomcat-9.0.36.tar.gz tomcat
```

## configurar variable de entorno path del sistema al fichero bashrc
```
vi .bashrc
```

### Agregar comando
```
export PATH=$PATH:/home/tomcat/tomcat/bin
```

### Cargar automaticamente la variable
```
. ./.bashrc // Esto hace que la variable se cargue
```

## Ver procesos java que estan funcionando
```
ps -ef | grep java
```

 
