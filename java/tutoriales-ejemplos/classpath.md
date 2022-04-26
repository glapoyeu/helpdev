Funcionamiento de CLASSPATH, package y jar
Introducción al CLASSPATH

Cuando empezamos con java es bastante habitual obtener el error "class not found" al intentar ejecutar nuestro programa, aunque estamos bastante seguros de que todo está como debía: existe el .class, he puesto bien el classpath, etc.

Aquí vamos a tratar de explicar por qué se nos producen estos errores y cómo están interrelacionados los directorios que ponemos en el classpath, también, por supuesto, qué es eso del classpath, cómo hemos construido nuestro fichero .jar (si es que lo hemos creado) y las directivas package que hemos puesto en nuestro código. La relación es bastante estrecha y el poner uno de ellos de una manera y otro de otra, hará que se nos produzca el error antes mencionado.
CLASSPATH

Vamos a suponer que tenemos un programa java compuesto por varias clases. En ninguna parte de nuestras clases hemos puesto package y lo tenemos todo compilado en un mismo directorio, por ejemplo /users/chuidiang/MI_PROYECTO  ( o C:\users\chuidiang\MI_PROYECTO en windows). Supongamos además que nuestro método main() está en el fichero ClasePrincipal.class.

Si estamos en ese directorio y ejecutamos nuestro programa java, escribimos algo así como

$ java ClasePrincipal

El comando java busca las clases que necesita en unos determinados directorios y ficheros .jar. Por defecto los buscará en los .jar propios de java y en el directorio en el que se esté ejecutando el comando java. Esto es cierto en la versión de java 1.4 y superiores. Para versiones anteriores es posible que no se busquen las clases en el directorio actual.

Si estamos en el directorio indicado, tenemos una versión de java moderna y repito, no hemos puesto package por ningún lado, todo esto funcionará correctamente.
 
¿Qué pasa si estamos en otro directorio?

Si estamos en otro directorio que no es el que contiene los ficheros .class de nuestro programa, o si estamos en el mismo directorio pero nuestra versión de java es antigua, entonces el comando java no los encontrará. Nuestros .class no están en los directorios en los que java busca por defecto.

Para que java los encuentre, tenemos dos opciones:

    Definir la variable de entorno CLASSPATH. Dándole a esta variable los directorio donde java debe buscar clases, todo irá de maravilla. Insisto, no hemos puesto package por ningún lado.

    En linux sería algo así como
    CLASSPATH=/users/chuidiang/MI_PROYECTO; export CLASSPATH
    java ClasePrincipal

    En windows
    set CLASSPATH=C:\users\chuidiang\MI_PROYECTO
    javaClasePrincipal
    Ejecutar java con la opción -cp (o -classpath), en la que se indica el directorio en el que están las clases.

    En linux

    java -cp /users/chuidiang/MI_PROYECTO   ClasePrincipal

    En windows

    java -cp C:\users\chuidiang\MI_PROYECTO   ClasePrincipal

¿Qué pasa si tengo mis clases repartidas en varios directorio?

Si nuestros ficheros .class están en varios directorios, el asunto es fácil. Basta poner todos los directorios seguidos con el separador adecuado (según el sistema operativo). Si los directorios son MI_PROYECTO/CLASES_1 y MI_PROYECTO/CLASES_2, sería

    Para linux, usando como separador el "dos puntos" ":"

    CLASSPATH=/users/chuidiang/MI_PROYECTO/CLASES_1:/users/chuidiang/MI_PROYECTO/CLASES_2
    export CLASSPATH
    java ClasePrincipal

    o bien

    java -cp /users/chuidiang/MI_PROYECTO/CLASES_1:/users/chuidiang/MI_PROYECTO/CLASES_2 ClasePrincipal
    Para windows, usando como separador el "punto y coma" ";" (El "dos puntos" se usa para poner después de la unidad de disco, por ejemplo, "C:\" )

    set CLASSPATH=C:\users\chuidiang\MI_PROYECTO\CLASES_1;C:\users\chuidiang\MI_PROYECTO/CLASES_2
    java ClasePrincipal

    o bien

    java -cp C:\users\chuidiang\MI_PROYECTO\CLASES_1;C:\users\chuidiang\MI_PROYECTO/CLASES_2 ClasePrincipal

Aquí es donde podemos empezar a tener problemas si no nos conocemos bien los trucos de todo esto. Hay dos detalles que debemos tener en cuenta:

    La opción -cp anula a la variable de entorno CLASSPATH. Es decir, si ejecutamos java con -cp, se ignora el contenido de CLASSPATH. Por ello, no podemos poner unos directorios en CLASSPATH y otros en -cp. Es incorrecto, por ejemplo

    CLASSPATH=/users/chuidiang/MI_PROYECTO/CLASES_1; export CLASSPATH
    java -cp /users/chuidiang/MI_PROYECTO/CLASES_2 ClasePrincipal

    Debemos poner todos los directorios implicados en sólo una de las dos opciones, o en CLASSPATH, o en -cp.
    Sólo se admite una opción -cp. Si ponemos varias, sólo se hará caso a una de ellas. Sería incorrecto, por ejemplo

    java -cp /users/chuidiang/MI_PROYECTO/CLASES_1 -cp /users/chuidiang/MI_PROYECTO/CLASES_2 ClasePrincipal
    Esto sólo encontraría las clases de uno de los directorios. Debemos poner todos los directorios en una sola opción -cp, separados por el "dos puntos" o el "punto y coma", según sea linux o windows.

PACKAGE

Todo lo contado ahora es sin poner package en ningún sitio de nuestro código.

Si hacemos, por ejemplo, una clase con package

package PAQUETE_1

public class MiClase
{
    ...
}

Esto "obliga" a que haya un directorio con el mismo nombre que el paquete (es decir, un directorio PAQUETE_1) y dentro de él el fichero .class correspondiente (es decir MiClase.class).

Una vez que lo tenemos organizado así, en classpath (me referiré ahora como classpath a cualquiera de las dos, la variable CLASSPATH o la opción -cp) tenemos que poner el directorio padre del directorio PAQUETE_1. Es decir, si tenemos /users/chuidiang/MI_PROYECTO/PAQUETE_1, en classpath debe ponerse /users/chuidiang/MI_PROYECTO.

Cuando alguna otra clase quiera utilizar esta clase, pondrá

import PAQUETE_1.MiClase;

y java buscará dentro de los directorios de classpath, un directorio llamado PAQUETE_1 y dentro de él la clase MiClase.class.

Todo esto puede llevar los niveles de "anidamiento" de paquetes/directorios que queramos. El classpath siempre será el del directorio que contiene al paquete de más alto nivel. A partir de él java empezará a buscar directorios que coincidan con los nombres de los paquetes. Si tenemos

package PAQUETE.SUB_PAQUETE.SUB_SUB_PAQUETE
...

java buscará dentro del directorio indicado en el classpath un directorio llamado PAQUETE y dentro de este un SUB_PAQUETE y dentro de ese un SUB_SUB_PAQUETE y dentro de este la clase.
JAR

 jar no es más que una versión específica de tar para java. Permite "empaquetar" en un único fichero .jar varias estructuras de directorios con sus ficheros.

Podemos considerar un fichero .jar como un árbol de directorios y ficheros (que ha sido empaquetado y no podemos navegar libremente por él, pero que es un árbol de directorios en todos los sentidos que nos afectan para classpath y package).
Sin package en el código

Si no hemos puesto package en nuestro código, debemos empaquetar sólo los .class, sin ningún tipo de directorio. En nuestro caso inicial de un sólo directorio sería fácil. Basta con irse a ese directorio y escribir

jar cvf MiLibreria.jar *.class

 Esto mete en MiLibreria.jar todas las clases del directorio actual. Las opciones cvf son para indicar que debe crear el fichero MiLibreria.jar (opción c), que nos vaya indicando lo que hace (opción v de "verbose" en inglés) y la f que indica que detrás va el nombre del fichero .jar.

Si tenemos nuestras clases en varios directorios, debemos usar la opción -C para ir metiendo las clases

jar cvf MiLibreria.jar -C /users/chuidiang/MI_PROYECTO/CLASES_1 . -C/users/chuidiang/MI_PROYECTO/CLASES_2 .

La opción -C va seguida de dos parámetros: un directorio y un fichero. El directorio es en el que debe buscar el fichero a empaquetar y el fichero es el fichero que debe empaquetar. Si el fichero es "punto" ".", empaqueta todos los ficheros que encuentre en el directorio indicado. Al usar la opción -C, sólo se empaqueta el fichero, no se empaqueta ningún directorio.

Sin embargo sería incorrecto

jar cvf MiLibreria.jar /users/chuidiang/MI_PROYECTO/CLASE_1/*.class ...

puesto que metería todo el árbol de directorios hasta llegar a los .class y luego metería los .class dentro de esos directorios.

Una vez que tenemos el .jar, en el classpath debemos poner el fichero .jar con su path absoluto, separado de otros directorios de clases o ficheros .jar por el separador adecuado al sitema operativo. Como dijimos anteriormente, un .jar es como si fuera un (o varios) directorio empaquetado, por lo que podemos ponerlo en un classpath como un directorio más. Por ejemplo

java -cp /users/chuidiang/MiLibreria.jar:/users/chuidiang/OTRO_DIRECTORIO:/users/chuidiang/OtroFichero.jar ClasePrincipal
Con package en el código

Si hemos puesto package en nuestras clases, nos pasa lo mismo que antes. Debemos crear un directorio con el nombre del paquete y dejar en él nuestros .class. Luego con jar debemos empaquetar el directorio con las clases que tiene dentro

En el ejemplo de /users/chuidiang/MI_PROYECTO/PAQUETE_1/MiClase.class, debemos irnos a /users/chuidiang/MI_PROYECTO y ejecutar

jar cvf MiLibreria.jar PAQUETE_1/*.class

Esto mete dentro de MiLibreria.jar el directorio con todas sus clases. Al poner en el classpath este fichero .jar, todo irá correctamente, igual que antes. java busca en el fichero .jar un directorio con el mismo nombre que el paquete y dentro de él la clase. Por ello es importante empaquetar también el directorio.

Ojo, es incorrecto esto otro

jar cvf MiLibreria.jar /users/chuidiang/MI_PROYECTO/PAQUETE_1/*.class

porque metería en nuestro .jar también los directorios users y chuidiang. El package de nuestra clase debería ser users.chuidiang.MI_PROYECTO.PAQUETE_1.

Si no queremos irnos al directorio MI_PROYECTO para hacer el .jar, debemos usar nuevamente la opción -C.

jav cvf MiLibreria.jar  -C /users/chuidiang/MI_PROYECTO  PAQUETE_1

Esto metería el directorio PAQUETE_1 completo en el .jar.

Para verificar qué es lo que se ha metido en nuestro .jar y asegurarnos que es lo que queremos, se usa la opción t

jar tvf MiLibreria.jar

Esto saca un listado con todos los ficheros y directorios que hay en el fichero .jar. Al listar cada fichero .class, pone delante el directorio/directorios en los que está metido (y que debe coincidir con su package si todo es correcto). Si no es así, debemos rehacer el .jar
RESUMEN

Para que todo funciones correctamente:

    Usar, o la variable CLASSPATH, o una única opción -cp.
    Crear una estructura de directorios igual a la estructura de paquetes y subpaquetes. Cada .class debe estar en el directorio que indique su package.
    Poner en CLASSPATH o -cp el directorio padre de la estructura de directorios equivalente a la de paquetes. No poner directamente el directorio que contiene las clases, salvo que estas no contengan package.
    Empaquetar en los ficheros .jar las estructuras de directorios completas correspondientes a los package. Incluir el fichero .jar dentro del CLASSPATH o -cp.
