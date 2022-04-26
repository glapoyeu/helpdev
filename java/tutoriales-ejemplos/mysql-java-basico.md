Lo básico de MySQL con Java

Una vez instalada la base de datos, MySQL en nuestro ejemplo, vamos a ver de forma rápida como podemos conectarnos con ella y ejecutar los comandos SQL básicos: SELECT, INSERT, UPDATE y DELETE.

También, por si acaso, crearemos desde Java la tabla de base de datos que vamos a usar y la borraremos.
El driver con la base de datos

Lo primero que necesitamos para conectarnos con una base de datos es un Driver (o Connector) con ella. Ese Driver es la clase que, de alguna forma, sabe cómo hablar con la base de datos. Desgraciadamente (y hasta cierto punto es lógico), java no viene con todos los Drivers de todas las posibles bases de datos del mercado. Debemos ir a internet y obtener el Driver, normalmente en la página de nuestra base de datos

Para el caso de MySQL, podemos descargarlo de http://dev.mysql.com/downloads/connector/j/5.0.html. Nos bajamos el mysql-connector-java-5.0.5.zip, lo desempaquetamos en algún sitio y nos quedamos con el mysql-connector-java-5.0.5-bin.jar que viene dentro. En ese jar está la clase Driver que nos interesa.

Tendremos que poner ese jar accesible en nuestro proyecto. Dependiendo de qué utilicemos para programar en java hay muchas opciones.

    Guardarlo en la carpeta <DIRECTORIO_DE_JAVA>/jre/lib/ext. Si has hecho una instalación por defecto en Windows, estará en C:\Archivos de Programa\Java\jdk1.5.0_05\jre\lib\ext. Esta es la carpeta en la que java nos deja poner jar adicionales para que se encuentren por defecto. Es buena idea ponerlo aquí si vamos a hacer muchos programas con base de datos. Si solo vamos a hacer uno, es mejor no ponerlo aquí. Si decidimos usar esta carpeta, también debemos tener la precaución de ir retirando jars de versiones antiguas cuando pongamos las nuevas, o tendremos problemas.
    Ponerlo en la variable de entorno CLASSPATH, siempre que nuestro IDE (eclipse, netbeans, etc) haga caso de esa variable. Si trabajas con una ventana de ms-dos (símbolo del sistema), puedes poner esto

    C:\> set CLASSPATH=<PATH_DEL_JAR>\mysql-connector-java-5.0.5-bin.jar
    Añadirlo en nuestro IDE. En el ejemplo de eclipse, damos con el botón derecho del ratón sobre el proyecto, para sacar el menú y elegimos "properties"->"java build path"->"Libraries"->"Add external JARs..." y seleccionamos nuestro jar.

Conectarnos con la base de datos

Una vez que java tiene el jar accesible y sabe dónde encontrarlo, ya podemos empezar con el código. Lo primero es conectarse con la base de datos

El código puede ser como este

import java.sql.Connection;
import java.sql.DriverManager;
...
try
{
   Class.forName("com.mysql.jdbc.Driver");
   Connection conexion = DriverManager.getConnection("jdbc:mysql://localhost/agenda", "root", "LA_PASSWORD");
   ...

En primer lugar hemos hecho los import adecuados para poder obtener la conexión.

Luego hay que asegurarse de que se inicializa el Driver y se registra. Para ello hay dos opciones. Hacer un new de él o bien instanciarlo con Class.forName("nombre de clase"), que es como hacer el new, pero de una forma rara.

La ventaja de hacerlo con Class.forName() es que sólo necesitaremos el Driver de una base de datos si lo usamos. Me explico. Imagina que haces un programa que permite permite conectarse con varias bases de datos distintas: MySQL, PostGres, Oracle, etc. En algún sitio damos a elegir al usuario qué tipo de base de datos tiene.

Si hacemos new de los Driver, tendremos que hacer unos if para en función de la elegida, hacer new de uno o de otro. El problema es que en nuestro código estarán los import y los new de todos los Driver de todas las bases de datos necesaras, los vayamos a usar o no. El programa no compilará y no correrá si no tiene todos los Driver accesibles.

/* Todos estos import son necesarios para que compilen los news correspondientes */
import driver_de_mysql;
import driver_de_postgres;
import driver_de_oracle;
...
if (ha_elegido_mysql)
   new driver_de_mysql();
if (ha_elegido_postgres)
   new driver_de_postgres();
if (ha_elegido_oracle)
   new driver_de_oracle();

Sin embargo, con Class.forName() no necesitamos el import. Haremos los mismos if de antes, pero usaremos simplemente String distintos para llamar a Class.forName(). No necesitamos los import y no necesitamos los jar con los Driver, salvo el jar con el Driver que realmente elija el usuario. Es cosa suya tener instalado SU driver de SU base de datos.

/* No hacen falta imports */
...
if (ha_elegido_mysql)
   Class.forName("driver_de_mysql");
if (ha_elegido_postgres)
   Class.forName("driver_de_postgres");
if (ha_elegido_oracle)
   Class.forName("driver_de_oracle");

Una vez que nos hemos asegurado que java tiene el Driver cargado, simplemente pediremos conexión con la base de datos a la clase DriverManager.

Connection conexion = DriverManager.getConnection("jdbc:mysql://localhost/agenda", "root", "LA_PASSWORD");

DriverManager tiene muchos métodos getConnection() con parámetros variados. Todos son variantes de lo mismo y la información que suministramos es la misma. Aquí hemos utilizado uno con tres parámetros String, que vamos a explicar.

    url: Es una cadena que nos permite localizar la base de datos. Para mysql, el formato es "jdbc:mysql://ordenador_donde_corre_la_base_de_datos/nombre_base_datos". Donde se pone el nombre o IP del ordenador en el que se encuentra nuestro servidor de base de datos y el nombre de la base de datos. En nuestro ejemplo, tenemos el servidor de base de datos corriendo en el mismo ordenador que el programa java, por lo que ponemos localhost. La base de datos la he llamado agenda. El comando SQL para crear la base de datos agenda sería

    mysql> CREATE DATABASE agenda;
    user: Un usuario válido para la base de datos.
    password: La clave del usuario.

Si todo va bien, tendremos nuestra conexión a la base de datos. Si va mal, saltará una excepción. Por eso es importante (y obligatorio para que compile) meter todo esto en un try-catch.

Esta forma de obtener una conexión está bien para apliaciones sencillas, en el que únicamente se establece una conexión con la base de datos, no hay muchos hilos trabajando, etc. Si nuestra aplicación es algo más compleja/seria, en el que varios hilos pueden trabjar simultáneamente, en vez de obtner las conexiones directamente con DriverManager.getConnection(), es mejor obtenerlas a trav�s de un Pool de Conexiones.
Creamos la tabla en Base de Datos ... y la borramos.

Para enviar comandos SQL a la base de datos, se usa la clase Statement de java. Esta clase se obtiene a partir de la conexión, de esta forma:

Statement st = conexion.createStatement();

por supuesto, dentro de un try-catch.

Statement tiene muchos métodos, pero hay dos interesentantes: executeUpdate() y executeQuery(). El primero se usa para sentencias SQL que impliquen modificaciones en la base de datos (INSERT, UPDATE, DELETE, etc). El segundo sólo para consultas (SELECT y similares).

Nuestra creación de tabla afecta a la base de datos, así que con executeUpdate().

st.executeUpdate("CREATE TABLE contacto (id INT AUTO_INCREMENT, PRIMARY KEY(id), nombre VARCHAR(20), apellidos VARCHAR(20), telefono VARCHAR(20))");

Esto crea una tabla contacto con cuatro campos: id, nombre, apellidos y telefono.

Para borrar la tablita esta, lo mismo, pero con DROP en vez de CREATE.

st.executeUpdate("DROP TABLE contacto");
Insertar datos en la base de datos

Vamos a hacer un esfuerzo de imaginación y supongamos que no hemos borrado la tabla contacto, que sigue existiendo en base de datos. Vamos a meterle datos.

String nombres[]={"Juan","Pedro","Antonio"};
String apellidos[]={"Gomez","Lopez","Alvarez"};
String telefonos[]={"123","456","789"};
...
for (int i=0;i<nombres.length;i++)
   st.executeUpdate("INSERT INTO contacto (nombre, apellidos, telefono) VALUES ('"+nombres[i]+"','"+apellidos[i]+"','"+telefonos[i]+"' )");

también en un try-catch.

He puesto los datos a insertar en unos arrays. Tú puedes recogerlos de un sitio con más sentido. Por ejemplo, si es una agenda de contactos, lo mejor es pedírselos al usuario. Yo, por simplicidad y porque para el ejemplo va que chuta, los pongo en un array y con un bucle los voy insertando.

Fíjate que la sentencia INSERT se va componiendo a base de sumar cadenas. Sería más eficiente con un StringBuffer, pero así se simplifica algo el ejemplo.

Fíjate que en el INSERT metemos comillas simples para los valores de cadenas, es decir, en nombre, apellidos y telefono. El INSERT sería, de verdad, así

INSERT INTO contacto (nombre, apellidos, telefono) VALUES ('Juan', 'Gomez', '123');

Ahí se ven más claras las comillas simples para los datos que definimos como VARCHAR en la creación de la tabla, es decir, Juan, Gomez y 123. Por eso en el código las hemos puesto

...VALUES ('"+nombres[i]+"','"...

Si fuesen datos numéricos no harían falta, son cosas de SQL, pero tenemos que tener cuidado de componerlas bien.
Consultar datos de la base de datos

Vamos a hacer ahora una consulta de los datos que acabamos de insertar en la base de datos. Las consultas se hacen con executeQuery() y nos devolverán un ResultSet.

El ResultSet de alguna forma representa una conexión hacia los datos. En el ResultSet NO están todavía los datos. Según se los vayamos pidiendo, los irá trayendo de base de datos. Esto quiere decir que si una consulta devuelve muchos resultados, no se nos va a llenar la memoria por el hecho de hacer la consulta.

Para traer el primer resultado, debemos llamar el método next() del ResulSet. Para el siguiente otro next() y así sucesivamente hasta que next() devuelva false, indicando que ya no quedaban datos. El código puede ser así

ResultSet rs = st.executeQuery("SELECT * FROM contacto");
while (rs.next())
{
   System.out.println("nombre="+rs.getObject("nombre")+
      ", apellidos="+rs.getObject("apellidos")+
      ", telefono="+rs.getObject("telefono"));
}
rs.close();

Hemos hecho un SELECT para obtener los datos. Luego un bucle while(rs.next()), es decir, mientras next() vaya trayendo resultados.

Después de next(), el resultado recién traido está disponible en el ResulSet. La forma de recoger los campos es pedirlos con algún método get(). Si sabemos de qué tipo es el dato, podemos pedirlo con getInt(), getString(), etc. Si no lo sabemos o nos da igual (como en este caso), bastará con un getObject(), capaz de traer cualquier tipo de dato.

En estos métodos get() podemos pasar como parámetro un entero, empezando en 1, que es el número del campo en el SELECT. Es decir, si hacemos SELECT campo1, campo2, campo3 ..., si pedimos getObject(1) obtenemos el valor de campo1, para getObject(2) el de campo2, etc.

Otra opción que a mi me gusta más, es pasar el nombre del campo, como se muestra en el código. Así, en el ejemplo de antes, getObject("campo1") nos devuelve el valor del campo1.
Update en la base de datos

Para UPDATE usamos executeUpdate(). Vamos a cambiar el número de teléfono de Juan. Ya que en la tabla hemos puesto un campo id, vamos primero a obtener el id de Juan para luego usarlo en el UPDATE. Esto no tiene por qué ser así, es sólo el ejemplo.

Para obtener el id:

rs = st.executeQuery("SELECT id FROM contacto WHERE nombre='Juan'");
rs.next();
int id = rs.getInt("id");

Es un SELECT para obtener el campo id de Juan. Con rs.next() hacemos que venga el primer resultado de la consulta (y único en nuestro ejemplo). Como sabemos que es un INT, lo recogemos con getInt() y lo guardamos.

Para hacerlo bien, deberíamos ver si rs.next() devuelve o no resultado (quizás no haya nadie que se llame Juan en la base de datos) o si devuelve más de uno (más de un Juan en la base de datos), pero no vamos a complicarnos la vida en este ejemplo sencillo.

Ahora, sabiendo el id, podemos hacer el UPDATE.

st.executeUpdate("UPDATE contacto SET telefono='111' WHERE id="+id);

Esta vez, como el campo id es numérico, no hemos puesto las comillas simples.
Borrar datos de la base de datos

El borrado se hace con DELETE y executeUpdate(). Vamos a borrar a Pedro, que nos hemos peleado con él. Obtenemos primero su id, igual que antes.

rs = st.executeQuery("SELECT id FROM contacto WHERE nombre='Pedro'");
rs.next();
id = rs.getInt("id");

y ahora lo borramos

st.executeUpdate("DELETE FROM contacto WHERE id="+id);
Cerramos conexiones

Los Connection, Statement y ResultSet con conexiones abiertas con base de datos. Debemos cerrarlas.

ResultSet se cierra solo cuando hacemos otra llamada execute() al Statement del que obtuvimos este ResultSet o bien cuando el recolector de basura "recolecta" al ResultSet. No nos preocupa en principio que se quede abierto, porque se acabará cerrando solo.

Eso sí, no podemos hacer nada con el Statement hasta que hayamos terminado con el ResultSet o se nos cerrará. Si necesitamos realizar otra cosa con base de datos, debemos crear otro Statement distinto.

Cuando terminemos con el Statement, lo cerramos con st.close().

Cuando terminemos con la Connection, la cerramos con conexion.close().

Vamos ahora a ver un ejemplo tonto completo de consulta a mysql con java.