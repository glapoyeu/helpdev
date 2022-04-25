1) Comando ls: Saber el tamaño de un directorio

Al utilizar el comando **ls**, de forma predeterminada, nos indica el tamaño de una carpeta:

Ejemplo:
```
[root@LINUX1 SUBDIR]# ls -lFh

total 121M
-rw-r--r-- 1 root root 24M Aug 25 22:15 SYSADMIT.file01
-rw-r--r-- 1 root root 24M Aug 25 22:16 SYSADMIT.file02
-rw-r--r-- 1 root root 24M Aug 25 22:16 SYSADMIT.file03
-rw-r--r-- 1 root root 24M Aug 25 22:16 SYSADMIT.file04
-rw-r--r-- 1 root root 24M Aug 25 22:16 SYSADMIT.file05
```
 
Si nos fijamos, en el color verde, veremos el total.

Los parámetros utilizados, son los siguientes:

```
-l Formato largo.  
-F: Clasificación.  
-h: Formato humano (Muestra MB, GB, etc...)  
```

El problema de esta opción, es que para saber cuanto ocupa cada directorio, deberemos entrar uno por uno, listar y obtener el total.

2)  Comando du: Saber el tamaño de un directorio

El comando para saber el tamaño de un directorio o bien una estructura de directorios es el comando: **du (disk usage).**

Veamos algunos ejemplos de su funcionamiento:

A) Ejecutamos: **du -h**
```
[root@LINUX1 SYSADMIT]# du -h
121M    ./SUBDIR3
73M     ./SUBDIR2
49M     ./SUBDIR1/SUB1
169M    ./SUBDIR1
361M    .
```
 
Nos muestra la suma desde el directorio actual y todos sus subdirectorios, en este caso: 361MB

También nos aparece lo que ocupa cada directorio.

Con el parámetro **-h**, conseguimos que los resultados aparezcan en MB, GB, etc..

B) Ejecutamos: **du -sh**
```
[root@LINUX1 SYSADMIT]# du -sh
361M    .
```

En este ejemplo, utilizando el parámetro **-s**, conseguimos que solo nos muestre el total, contando desde el directorio actual y todos los subdirectorios.

C) Ejecutamos: **du -ha**

```
[root@LINUX1 SYSADMIT]# du -ha
25M     ./SUBDIR3/SYSADMIT.file05
25M     ./SUBDIR3/SYSADMIT.file04
25M     ./SUBDIR3/SYSADMIT.file01
25M     ./SUBDIR3/SYSADMIT.file03
25M     ./SUBDIR3/SYSADMIT.file02
121M    ./SUBDIR3
25M     ./SUBDIR2/SYSADMIT.file01
25M     ./SUBDIR2/SYSADMIT.file03
25M     ./SUBDIR2/SYSADMIT.file02
73M     ./SUBDIR2
25M     ./SUBDIR1/SYSADMIT.file05
25M     ./SUBDIR1/SYSADMIT.file04
25M     ./SUBDIR1/SUB1/SYSADMIT.file01
25M     ./SUBDIR1/SUB1/SYSADMIT.file02
49M     ./SUBDIR1/SUB1
25M     ./SUBDIR1/SYSADMIT.file01
25M     ./SUBDIR1/SYSADMIT.file03
25M     ./SUBDIR1/SYSADMIT.file02
169M    ./SUBDIR1
361M    .
```
 
Con el parámetro **-a**, conseguimos el detalle de tamaño de cada fichero, además de los totales.

En verde, el tamaño total.
En naranja, el tamaño de los directorios y subdirectorios.
El resto, el tamaño de cada fichero.

D) Ejecutamos: **du -h --max-depth=1**

```
[root@LINUX1 SYSADMIT]# du -h --max-depth=1
121M    ./SUBDIR3
73M     ./SUBDIR2
169M    ./SUBDIR1
361M    .
```
```
[root@LINUX1 SYSADMIT]# du -h
121M    ./SUBDIR3
73M     ./SUBDIR2
49M     ./SUBDIR1/SUB1
169M    ./SUBDIR1
361M    .
```

Con el parámetro, **--max-depth**, podemos indicar el nivel de profundidad de directorios que queremos que nos muestre.

Si indicamos **--max-depth**, a 1, solo nos mostrará un nivel de directorios partiendo del directorio actual.

De todas formas, la suma del total de ficheros ubicados en los subdirectorios, se cuenta de igual forma.

Si nos fijamos en el ejemplo:

En verde, el tamaño total: El valor del tamaño total es el mismo indicando el parámetro **--max-depth**, a 1 o no.

En naranja, se muestra uno de los subdirectorios situado en un segundo nivel, pero en cambio, indicando el parámetro  **--max-depth**, a 1, ya no aparece.

E) Ejecutamos:  **du -h| sort -h // du -h| sort -h -r**
```
[root@LINUX1 SYSADMIT]# du -h| sort -h
49M     ./SUBDIR1/SUB1
73M     ./SUBDIR2
121M    ./SUBDIR3
169M    ./SUBDIR1
361M    .
```
```
[root@LINUX1 SYSADMIT]# du -h| sort -h -r
361M    .
169M    ./SUBDIR1
121M    ./SUBDIR3
73M     ./SUBDIR2
49M     ./SUBDIR1/SUB1 
```

El comando sort, dispone de un parámetro para ordenar el formato de salida **du -h** (MB, GB, etc) que es utilizando también el parámetro -h.

Por defecto, **sort -h**, ordena de menor a mayor, si queremos que ordene de mayor a menor, deberemos ejecutar: **sort -h -r**.

---

Si lo que queremos es saber el tamaño ocupado a nivel de volumen, podemos utilizar el comando **df**:

Para saber el tamaño que ocupa una carpeta en el disco a través de terminal vamos a usar el comando «du» que ya hemos visto con anterioridad para otros temas, pero en esta ocasión veremos como hacer que nos de información sobre las carpetas y subcarpetas.

- Opción 1:
```
du -shc Descargas
```
Nos da cuanto ocupa el total de la carpeta.

- Opción 2:
```
du -shc Descargas/*
```
Nos deja ver el tamaño de cada archivo o carpeta junto con el total al final.

Los parámetros explicados

    -a: saca la información de los archivos además de las carpetas.
    -c: informa también del total de los directorios listados, además del tamaño de cada carpeta.
    -h: indica el tamaño en formato legible (K, M, G…) en vez de en sectores de disco.
    -s: saca solo el total, en vez del tamaño de cada subcarpeta.

Para ordenar la lista podemos hacer:
```
du -sh Descargas/* | sort -h
```