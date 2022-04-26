# Connection pool

En computación, se denomina connection pool (agrupamiento de conexiones) al manejo de una colección de conexiones abiertas a una base de datos de manera que puedan ser reutilizadas al realizar múltiples consultas o actualizaciones.

Cada vez que un programa cliente necesita comunicarse con una base de datos, establece una conexión, generalmente utilizando un protocolo especializado. Esta conexión, generalmente se mantiene abierta el tiempo que dura la ejecución del programa y sólo es cerrada al finalizar el trabajo de la aplicación con la base de datos.

Este esquema, sin embargo, no es apropiado para aplicaciones multitarea en las que el mismo programa puede querer realizar en paralelo más de una operación sobre la base de datos. Este caso es típico de las aplicaciones que proveen servicio de páginas web a múltiples usuarios; en ellas, el número de operaciones sobre la base de datos y su cadencia dependen de la actividad de los usuarios de las páginas servidas.

Al mantenerse abierto un grupo de conexiones, éstas son atribuidas a los diferentes hilos de ejecución únicamente el tiempo de una transacción con la base de datos. Al finalizar su utilización, la conexión se pone a disposición de otro hilo de ejecución que necesite de ese recurso, en lugar de cerrarla o de asignarla permanentemente a un único hilo de ejecución.

Según las políticas de agrupamiento de conexiones, cuando todas están en uso se establecen nuevas conexiones, y si ello no es posible, se deja el hilo de ejecución en espera de la liberación de alguna conexión. A la inversa, si pasa mucho tiempo sin que se utilicen las conexiones, algunas de ellas o todas podrían ser cerradas