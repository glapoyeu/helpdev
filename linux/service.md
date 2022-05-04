# Entendiendo los servicios en Linux
Linux ofrece multitud de servicios o servidores, estos pueden iniciar o arrancar junto con la carga del sistema o pueden después ser puestos a funcionar cuando se requieran (es lo mejor). Parte esencial de la administración de sistemas Linux es continuamente trabajar con los servicios que este proporciona, cosa que es bastante sencilla. En este tutorial aprenderás todo lo necesario sobre como inicar/detener/reinicar etc. los servicios de tu equipo GNU/Linux.

## Iniciando servicios manualmente, directorio init.d

Dentro de esta carpeta ubicada en /etc o en /etc/rc.d dependiendo de la distribucción, se encuntran una serie de scripts que permiten inicar/detener la gran mayoría de los servicios/servidores que estén instalados en el equipo. Estos scripts están programados de tal manera que la mayoría reconoce los siguientes argumentos:

- start
- stop
- restart
- status 

Los argumentos son autodescriptivos, y tienen permisos de ejecucción, entonces siendo root es posible iniciar un servicio de la siguiente manera, por ejemplo samba:

```
#> /etc/rc.d/init.d/smb start
Starting Samba SMB daemon           [OK]
```

Solo que hay que cambiar start por stop | restart | status para detenerlo, reiniciarlo (releer archivos de configuración) o checar su estatus. Ahora bien si estás parado dentro del directorio puedes hacerlo asi.

```
#> pwd
/etc/rc.d/init.d
#> ./smb stop
Shutting down Samba SMB daemon     [OK]
```

Se trata de tan solo un script asi que con el permiso de ejecucción (x) puedes ejecutarlo con ./ seguido del nombre del servicio, sin espacios y después el argumento que necesites, inicarlo, detenerlo etc.

## El comando service

En varias distros, como Centos, Fedora o RedHat, existe el comando service, este comando permite también iniciar y/o detener servicios, de hecho funciona exactamente igual a como si escribieramos la ruta completa hacía el directorio init.d, con service se indica de la siguiente manera:

```
#> service mysql status
Checking for service MySQL: stopped
```

Si se desea iniciarlo:

```
#> service mysql start
Starting service MySQL                               [OK]
```

niciando servicios desde el arranque del sistema

En muchos casos es conveniente que un servidor o servicio inicien junto con el arranque del equipo en si, por ejemplo el servidor web Apache o alguna base de datos, esto es para que estén disponibles todo el tiempo y no se requiera de intervención del administrador para iniciarlos.

En Linux, a diferencia de otros sistemas operativos, es posible configurarlo en base a niveles de ejecucción (run levels), cada nivel de ejecucción (en la mayoría de las distros son 7), inicia o detiene (Start o Kill) ciertos servicios. Estos niveles son los siguientes:

- 0 Detener o apagar el sistema
- 1 Modo monousuario, generalmente utilizado para mantenimiento del sistema
- 2 Modo multiusuario, pero sin soporte de red
- 3 Modo multiusuario completo, con servicios de red
- 4 No se usa, puede usarse para un inicio personalizado
- 5 Modo multiusuario completo con inicio gráfico ( X Window)
- 6 Modo de reinicio (reset) 

Por ejemplo el nivel 0, que apaga el equipo, mata o detiene a todos los procesos del sistema, todos los servicios, lo mismo hace el 6 con la diferencia que después inicia un script que permite reiniciar el sistema. El nivel por omisión o por default del sistema esta definido en /etc/inittab, en la línea initdefault:

```
$> grep initdefault /etc/inittab
id:5:initdefault:
# runlevel 0  is  System halt   (Do not use this for initdefault!)
# runlevel 6  is  System reboot (Do not use this for initdefault!)
```

La salida es de una distro OpenSuse 10. Como se puede apreciar existe una línea que comienza con id, seguido por 5 que indicaría entonces el nivel por default del sistema, basta con cambiar este valor con cualquier editor, reiniciar el equipo y se entraría a otro nivel, por ejemplo el 3 que tiene una funcionalidad completa pero sin sistema de ventanas X Window (ideal para equipos obsoletos o con pocos recursos de hardware). Nótese que en el listado previo aparecen dos líneas de comentarios, las que empiezan con #, que hacen la advertencia de no usar nunca los niveles 0 y 6 de initdefault, ya que jamás arrancaría el equipo. Se tendría que usar un método de rescate. (He aqui la importancia de jamás dejar una terminal abandonada con una sesión de root abierta ya que podría llegar un bromista y rápidamente cambiar la línea initdefault a 6 por ejemplo, y después estaría muy divertido viendo a un pobre administrador novato trátandose de explicar porque diablos el equipó se resetea hasta el infinito sin nunca entrar a una pantalla de login).

El nivel de ejecucción actual del sistema puede ser consultado con who -r y con el comando runlevel, este último muestra dos números el primero es el nivel previo en que se estaba y el segundo el nivel actual, si no se ha cambiado de nivel aparece una N.

## Comando init

Este comando te permite cambiar el nivel de ejecucción actual por otro, es decir puedes estar en el nivel 3 y en cualquier momento pasar al 5 o viceversa, o entrar a un modo monousuario para tareas de mantenimiento como utilizar fsck. Esto sin necesidad de cambiar la línea initdefault en /etc/inittab, simplemente usando el comando init:

```
#> init 3
```

Puedes reiniciar el equipo con init 6 o incluso apagarlo con init 0, de hecho shutdown y otros comandos como halt, lo que hacen es invocar a init para apagar el equipo mas otras tareas como sincronizar buffers, etc. Si estás en una máquina que tenga particiones con sistemas de archivos ext2 o de windows como FAT, si invocas init 0 o init 6 tal cual, corres el riesgo de perder datos que no estén guardados en disco todavía, para esto combina init 0 con el comando sync que escribe lo que este en buffers de memoria a disco:

```
#> sync; sync; init 0    
```

Con un doble sync nos aseguramos que todo se escriba en disco antes de comenzar a apagar.

## Los directorios rc
Debajo de /etc se encuentra el directorio rc.d que a la vez contiene un directorio para cada nivel de ejecucción, asi tenemos rc0.d, rc1.d, rc2.d, rc3.d, etc. Hay algunas distros que estos directorios están ubicados directamente en /etc. Como ya te imaginaras, cada uno de estos directorios contiene scripts (o mas bien enlaces a scripts) que apuntan al directorio init.d, entonces el comando init (ya sea ejecutado manualmente o cuando se inicia el sistema), dependiendo del nivel indicado leera cada uno de los enlaces o accesos directos del directorio respectivo.

Ahora bien, un ejemplo (parcial) típico de estos directorios puede ser el siguiente, tomado de rc3.d

```
lrwxrwxrwx   1 root root    7 Oct 20 20:05 K22dbus -> ../init.d/dbus
lrwxrwxrwx   1 root root    9 Oct 20 20:05 K22resmgr -> ../init.d/resmgr
lrwxrwxrwx   1 root root    8 Oct 20 20:05 K24fbset -> ../init.d/fbset
lrwxrwxrwx   1 root root    9 Oct 20 20:05 K24random -> ../init.d/random
lrwxrwxrwx   1 root root    8 Oct 20 18:23 S01fbset -> ../init.d/fbset
lrwxrwxrwx   1 root root    9 Oct 20 18:22 S01random -> ../init.d/random
lrwxrwxrwx   1 root root    7 Oct 20 18:29 S03dbus -> ../init.d/dbus
lrwxrwxrwx   1 root root    9 Oct 20 18:23 S03resmgr -> ../init.d/resmgr
lrwxrwxrwx   1 root root   12 Oct 20 18:24 S04boot.udev -> ../init.d/boot.udev
lrwxrwxrwx   1 root root   10 Oct 20 21:16 S05network -> ../init.d/network
lrwxrwxrwx   1 root root    9 Oct 20 18:23 S06syslog -> ../init.d/syslog
```

Nótese que todos son enlaces al directorio init.d que como ya se vió previamente es donde realmente están ubicados los scripts de arranque de los servicios o servidores. También, todos los enlaces comienzan con K (kill) o con S (start), es pues fácil deducir que los que comienzan con K son scripts que recibirán el argumento stop y los que comienzan con S el de start, es decir, se inician. Esta gran simplicidad ofrece una enorme potencia al momento de configurar servicios ya que basta con agregar o quitar enlaces con el formato indicado para personalizar los niveles de ejecucción a nuestro gusto.

Después de la K o S sigue un número consecutivo, seguido generalmente del nombre del servicio que afectan, el número secuencial es simplemente el orden en que se leeran los scripts, primero los K comenzando con el 01 y hacía adelante y después los S. Entonces si por ejemplo no queremos que se inicie el samba en el nivel 3 bastaría con borrar su enlace en este directorio:

```
#> pwd
/etc/rc.d/rc3.d
#> rm S54smb
```

Y listo, el servidor samba ya no arrancaría cuando entremos en este nivel de ejecucción. Si por lo contrario lo que deseamos es iniciar (o apagar) un servicio, basta con crear su enlace en el directorio respectivo:

```
#> pwd
/etc/rc.d/rc5.d
#> ln -s /etc/rc.d/init.d/mysql S90mysql
#> ls -l S90mysql
lrwxrwxrwx   1 root root    9 Oct 20 18:23 S90mysql -> ../init.d/mysql
```

Con esto la siguiente vez que iniciemos el equipo o cambiemos a nivel 5 con init, también se iniciará el servidor de la base de datos MySQL. El número 90 es escogido al azar entre 01 y 99 es simplemente el orden en que serán iniciados o detenidos los servicios.

## Comando chkconfig

Lo anterior puede resultar engorroso para más de alguno pero es la única manera si no se tienen herramientas gráficas o de línea de comandos para configurar los servicios mas agilmente, y precisamente una de estas herramientas de línea de comandos es chkconfig que permite configurar que servicios arrancan/detienen en cada nivel de ejecucción. Aclaración: chkconfig no inicia ni detiene servicios al momento (excepto aquellos bajo xinetd), tan solo crea o elimina precisamente los enlaces de los que se habló en el punto anterior de una manera mas amigable. Si lo que se quiere es iniciar o detener el servicio en tiempo real o manualmente hay que usar service o directamente el script con su argumento conveniente tal como se explicó anteriormente.
Veamos ejemplos que muestran como trabajar con chkconfig.

Con la opción --list nos da una lista completa de todos los servicios instalados y para cada nivel si arrancará (on) al entrar a ese nivel o se detendrá (off) o simplemente no se iniciara. Nótese que al final de la lista vienen los servicios que dependen del superservidor xinetd.

```
#> chkconfig --list
apache2                   0:off  1:off  2:off  3:off  4:off  5:off  6:off
bluetooth                 0:off  1:off  2:off  3:off  4:off  5:off  6:off
cron                      0:off  1:off  2:on   3:on   4:off  5:on   6:off
cups                      0:off  1:off  2:on   3:on   4:off  5:on   6:off
dhcpd                     0:off  1:off  2:off  3:off  4:off  5:off  6:off
ldap                      0:off  1:off  2:off  3:off  4:off  5:off  6:off
mysql                     0:off  1:off  2:off  3:off  4:off  5:off  6:off
named                     0:off  1:off  2:off  3:off  4:off  5:off  6:off
network                   0:off  1:off  2:on   3:on   4:off  5:on   6:off
nfs                       0:off  1:off  2:off  3:on   4:off  5:on   6:off
quotad                    0:off  1:off  2:off  3:off  4:off  5:off  6:off
smb                       0:off  1:off  2:off  3:on   4:off  5:on   6:off
vmware                    0:off  1:off  2:on   3:on   4:off  5:on   6:off
xinetd                    0:off  1:off  2:off  3:on   4:off  5:on   6:off
zebra                     0:off  1:off  2:off  3:off  4:off  5:off  6:off
xinetd based services:
        rsync:              off
        sane-port:          off
        servers:            off
        services:           off
        swat:               on
        systat:             off
        tftp:               off
        time:               off
        time-udp:           off
        vnc:                off
        vsftpd:             off
```

Si se indica como argumento el nombre de algún servicio nos regresará su estatus:

```
#> chkconfig smb
smb   on
```

Sin ningún argumento nos dará una lista con el status de todos los servicios:

```
#> chkconfig
httpd  off
mysql  on
smb    on
```

Se desea que el servidor web apache (httpd) inicie cuando se entra en el nivel 5, entonces usamos la opción --level:

```
#> chkconfig --level 5 httpd on
```

La base de datos MySQL no se desea que inicie en los niveles 3 y 5, solo hasta que el administrador decida arrancarla:

```
#> chkconfig --level 35 mysql off
```

## Ambientes gráficos

En todas las distribucciones actuales en sus ambientes gráficos X Window incluyen herramientas de configuración se servicios que facilitan mucho la administración de servidores. En una misma pantalla podemos cambiar niveles de ejecucción, con un click indicar si el servicio arranca o se detiene, incluso iniciarlos o detenerlos en ese momento, todo vía gráfica. Lo mejor es aprenderlo a realizarlo desde la línea de comandos ya que esta será muy similar en casi todas las distros o puede ser que el linux que se esté utilizando no tenga servicios gráficos, aun asi, ya familiarizados con una distribucción es muy sencillo e intuitivo utilizar estas herramientas gráficas por lo que no entraremos en detalles, solo diremos que por ejemplo en SuSE con yast o yast2 es posible no solo controlar servicios sino multitud de cosas mas, en redhat tenemos redhat-config-services y en fedora e incluso versiones enterprise de redhat es system-config-services, mandrake, ubuntu, etc. Todas tienen una interfaz gráfica, es solo cuestión de buscarla.

Por otro lado tenemos interfaces web como la excelente herramienta webmin, que también permite un control total sobre el manejo de servidores, sumamente eficaz cuando se controlan servidores via remota por la web.


## Agradecimiento
- [linuxtotal](https://www.linuxtotal.com.mx/index.php?cont=info_admon_003)

## Consultas
- **Pregunta:** Ahora tengo una consulta. Estoy usando centos 9 y no tengo disponible el comando `chkconfig`, intenté instalarlo con apt-get install chkconfig pero no ecuentra el paquete. Alguna idea?

- - **Respuesta:** Este viene del futuro, a nivel de aclaración Rolly el comando apt, apt-get o aptiude son de Debian o Forks, Si usas Centos es una distribución basada en Red-Hat por ende deberás utilizar los comandos apropiados para ella, yum en vez de apt-get, no obstante y leyendo tu comentario, si lo que te pone es que no encuentra el paquete muy posiblemente estés trabajando con Debian 9 y te has confundido al catalogar la distribución, si es así lo que tienes que hacer primero habilitar los repositorios de debían 9.
nano /etc/apt/sources.list

Después compruebas que coincide con esto:

### Repositorio base stable.
deb http://ftp.es.debian.org/debian/ stretch main contrib non-free
deb-src http://ftp.es.debian.org/debian/ stretch main contrib non-free

### Repositorio de actualizaciones de seguridad
deb http://security.debian.org/debian-security stretch/updates main contrib non-free
deb-src http://security.debian.org/debian-security stretch/updates main contrib non-free

### Repositorio de actualizaciones anteriormente conocido como "Volatil"
deb http://ftp.es.debian.org/debian/ stretch-updates main contrib non-free
deb-src http://ftp.es.debian.org/debian/ stretch-updates main contrib non-free

### Repositorio multimedia
deb http://www.deb-multimedia.org stretch main contrib non-free

Con las teclas control + o y control + x grabas el fichero y sales del editor.
Después actualizas las claves pgp del repositorio multimedia con la siguiente orden:
Para descargarla:
wget http://www.deb-multimedia.org/.../deb-multimedia-keyring...

Para instalarla:
dpkg -i deb-multimedia-keyring_2016.8.1_all.deb

ahora actualizas la lista de paquetes y repositorio con:
apt-get update

y listo ya puedes buscar el paquete que no encontrabas 
