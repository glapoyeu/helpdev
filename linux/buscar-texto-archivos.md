# Cómo buscar archivos y texto dentro de estos en una terminal Linux

Son muchos los usuarios que poseen un sistema Linux. Sin lugar a dudas, el uso de la terminal para realizar acciones es algo tabú para muchos usuarios. Sin embargo, es realmente potente y es recomendable aprender, sobre todo por la rapidez para realizar diferentes acciones. En este artículo, os vamos a enseñar a buscar archivos y encontrar texto en el interior de los archivos.

Y es que a muchos usuarios la ausencia de una interfaz gráfica provoca cierto terror. Al final, todo radica en conocer los comandos necesarios. De esta forma se podrán recoger los resultados esperados. Podéis visitar nuestro tutorial sobre [mRemoteNG](https://www.redeszone.net/tutoriales/servidores/mremoteng-terminal-avanzado-windows/) que es un terminal avanzado para Linux.

Buscar un archivo es algo que en más de una ocasión los usuarios necesitan. Encontrar contenido dentro de los archivos no resulta algo tan habitual, pero también nos puede resultar útil.

## Cómo buscar archivos en Linux

Para realizar esta operación utilizando la terminal, todo radica en utilizar el comando **find**. Tal y como vamos a ver a continuación, existen algunas opciones que podemos considerar interesantes y que hay que tener en cuenta.

Sin ir más lejos, si queremos buscar un archivos deberíamos utilizar:


    find / -name «nombre del archivo»

Hay que destacar que la utilización de esta instrucción implica que se tengan en cuenta tanto mayúsculas como minúsculas. Por lo tanto, podría decirse que el uso de esta opción no es del todo aconsejable, sobre todo si no conocemos el nombre exacto.

La instrucción anterior quiere decir: se buscarán los archivos que correspondan con «nombre del archivo», teniendo en cuenta las mayúsculas y en el directorio / en todos sus subdirectorios.

Para no tener en cuentas las mayúsculas y minúsculas:

    find / -iname «nombre del archivo»

También se puede indicar que solo se busque en el directorio actual utilizando «.» :

    find . -iname «nombre del archivo»

¿Y si no conocemos el nombre exacto del archivo? No pasa absolutamente nada, se puede jugar con el «*» tanto antes como después de la aproximación del nombre del archivo:

    find /home/redeszone -iname «*.conf»

O bien:

    find /home/redeszone -iname «*configu*»

¿Y si quiero realizar el filtrado de los archivos? Tampoco hay ningún problema, ya se puede llevar a cabo un filtrado sobre el tipo de archivo que se muestra en los resultado. Por ejemplo, si queremos mostrar enlaces simbólicos, el comando a utilizar sería:

    find / -type f -iname «nombre del archivo»

Para archivos regulares:

    find / -type f -iname «nombre del archivo»

Y para carpetas:

    find / -type f -iname «nombre del archivo»

Tal y como se puede observar, se trata de un comando fácil de utilizar y que se puede completar de forma sencilla con diferentes atributos.

También se puede utilizar **locate**, aunque será necesario instalarlo, ya que por defecto no es habitual que se encuentre en el sistema. Una vez instalado, ¿cómo lo podemos utilizar? A continuación os dejamos un ejemplo. El comando básico sería el siguiente:

    locate -i «*.pdf»

Como podéis observar, disponemos de dos alternativas. Bajo nuestro punto de vista, preferimos hacer uso de find, aunque para gustos los colores.

## ¿Y si quiero buscar contenido en el interior?

Aunque parezca mentira, el comando que debemos utilizar para esto es **grep**. Un ejemplo de su uso sería el siguiente:

    grep -r -i «palabra o palabras» /home/redeszone/

Para que se entienda fácil, explicamos que significa cada uno de los elementos del comando. «-r» indica que se debe realizar de forma recursiva en todos los subdirectorios contenidos en la ubicación proporcionada. «-i» permite omitir las mayúscula y minúsculas, haciendo que la búsqueda sea mucho más sencilla. «palabra o palabras» es el texto que queremos probar. «/home/redeszone» es la ruta sobre la que se realizará la búsqueda.

Tal y como se puede observar, buscar archivos y contenido dentro de los mismos no es un problema, y con find y grep podemos encontrar los archivos o aquellos que contienen el texto que deseamos.