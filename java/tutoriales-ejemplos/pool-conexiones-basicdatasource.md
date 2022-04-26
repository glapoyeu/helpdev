# Pool de conexiones: BasicDataSource

Hemos visto en [mysql-java-basico](mysql-java-basico.md) que podemos establecer la conexión con la base de datos por medio de la clase **DriverManager.getConnection()**. De esta forma obtenemos una Connection real con la base de datos, es decir, al llamar a este método, se establece la conexión con la base de datos y cuando llamemos al método **connection.close()**, dicha conexión se cierra.

Sin embargo, hay un pequeño problema con esto. Varios hilos no pueden usar una misma conexión física con la base de datos simultáneamente, ya que la información enviada o recibida por cada uno de los hilos se entremezcla con la de los otros, haciendo imposible una escritura o lectura coherente en dicha conexión.

Hay varias posibles soluciones para este problema:

1. Abrir y cerrar una conexión cada vez que la necesitemos. De esta forma, cada hilo tendrá la suya propia. Esta solución en principio no es eficiente, puesto que establecer una conexión real con la base de datos es un proceso costoso. Andar abriendo y cerrando conexiones con frecuencia puede hacer que nuestro programa vaya más lento de lo debido.
2. Usar una única conexión y sincronizar el acceso a ella desde los distintos hilos. Esta solución es más o menos buena, pero requiere cierta disciplina al codificar, ya que tendremos que acordarnos de poner siempre los [synchronized](sincronizar_hilos_java.md) antes de hacer cualquier transacción con la base de datos. También tiene la pega de que los hilos deben esperar unos por otros
3. Finalmente, podemos tener varias conexiones abiertas, de forma que cuando un hilo necesite una, la "reserve" para su uso y cuando termine, la "libere" para que pueda ser usada por los demás hilos, todo ello sin abrir y cerrar la conexión cada vez. De esta forma, si hay conexiones disponibles, un hilo no tiene que esperar a que otro acabe. Esta solución es en principo la ideal y es lo que se conoce como un [pool de conexiones](connection-pool.md). Afortunadamente, no debemos preocuparnos de codificarlo nosotros mismos, ya que los hay disponibles en internet. Esta es la solución que vamos a detallar en este tutorial.

 
## Pool de conexiones

En java, un pool de conexiones es una clase java que tiene abiertas varias conexiones a base de datos. Cuando alguien necesita una conexión a base de datos, en vez de abrirla directamente con DriverManager.getConnection(), se la pide al pool usando su método pool.getConnection(). El pool coge una de las conexiones que ya tiene abierta, la marca como que alguien la está usando para no dársela a nadie más y la devuelve. La siguiente llamada a este método pool.getConnection(), buscará una conexión libre para marcarla como ocupada y la devolverá ... y así sucesivamente.

Cuando el que ha pedido la conexión termina de usarla, normalmente después de una transacción con la base de datos o varias seguidas, llama al método connection.close(). Esta conexión que nos ha sido entregada por el pool, realmente no se cierra con esta llamada. El método close() únicamente avisa al pool que ya hemos terminado con la conexión, de forma que sin cerrarla, la marca como libre para poder entregársela a otro que lo pida.

El uso del pool y de la conexión, por tanto, sigue el siguiente esquema

```
// Se le pide al pool una conexion libre
Connection conexion = pool.getConnection();

// Se hace una o más transacciones: select, update, insert, delete ...

// Se libera la conexión para su posible uso por otro hilo
conexion.close();
``` 

## apache dbcp : BasicDataSource

En la librería de *apache commons-dbcp* tenemos una implementación sencilla de un pool de conexiones. Esta librería necesita a su vez la librería [commons-pool(https://commons.apache.org/proper/commons-pool/downloads.html)], por lo que debemos descargarnos también esta. Una vez descargadas esas dos librerías y el driver de nuestra base de datos [mysql-connector-java-5.1.6.jar](https://dev.mysql.com/downloads/connector/j/5.1.html) para nuestro ejemplo, debemos añadir todas ellas como dependencias de nuestro proyecto o al [CLASSPATH](classpath.md) del mismo.

Una vez configurado todo, para usar BasicDataSource no tenemos más que hacer un new de esa clase y pasarle los parámetros adecuados de nuestra conexión con los métodos set() disponibles para ello. El siguiente trozo de código muestra cómo hacerlo

```
BasicDataSource basicDataSource = new BasicDataSource();
basicDataSource.setDriverClassName("com.mysql.jdbc.Driver");
basicDataSource.setUsername("hibernate");
basicDataSource.setPassword("hibernate");
basicDataSource.setUrl("jdbc:mysql://localhost/hibernate");

// Opcional. Sentencia SQL que le puede servir a BasicDataSource
// para comprobar que la conexion es correcta.
basicDataSource.setValidationQuery("select 1");
```

Símplemente hemos hecho el new de la clase y le hemos pasado:

- Con **setDriverClassName()** el nombre de la clase que es el driver de nuestra conexión a la base de datos, MySQL en nuestro ejemplo
- con **setUsername()** un nombre de usuario válido para la base de datos (hibernate).
- con **setPassword()** la password de base de datos para dicho usuario (hibernate).
- con **setUrl()** la cadena de conexión con la base de datos, en nuestro caso en el mismo ordenador en que va a correr nuestro ejecutable (localhost) y el nombre de la base de datos (hibernate)

Estas cuatro son las estrictamente necesarias. [BasicDataSource permite configurar muchas otras cosas](https://commons.apache.org/proper/commons-dbcp/configuration.html), como cuantas conexiones debe abrir como máxima, cuántas puede tener abiertas que no se estén usando, etc. Y una característica interesante, se le puede pasar una consulta que BasicDataSource ejecutará para comprobar que la conexión con la base de datos es correcta. Esta consulta se pasa con setValidationQuery() y en el caso del ejemplo es un simple "select 1", que siempre devolverá resultado independientemente de las tablas y datos existentes en la base de datos.

Una vez instanciado el BasicDataSource y para evitar que dentro de lo posible el resto de nuestro código dependa de este pool de conexiones concreto, se suele asignar el BasicDataSource a una variable de tipo **javax.sql.DataSource**, que es una interface estándar de java.

```
DataSource dataSource = basicDataSource;
// la variable dataSource es la que debería ver el resto de nuestro programa.
```

Una vez guardado nuestro dataSource en una variable de tipo DataSource, el código para hacer una transacción con la base de datos debe serguir este esquema:
```
Connection conexion = dataSource.getConnection();
// Hacer el select, insert, update, delete...
conexion.close();
```

Por ejemplo, si suponemos una tabla Person con cuatro campos id, edad, nombre y apellido, para hacer una inserción haríamos lo siguiente:
```
Connection conexion = null;
try {
   // BasicDataSource nos reserva una conexion y nos la devuelve.
   conexion = dataSource.getConnection();

   // La insercion.
   Statement ps = conexion.createStatement();
   ps.executeUpdate(
      "insert into person values (null,22,'Pedro','Martinez')");
} catch (SQLException e) {
   e.printStackTrace();
} finally {
   // En realidad no cierra la conexion, sino que avisa al pool de que
   // esta conexión queda libre.
   conexion.close();
}
```

Hemos puesto el **conexion.close()** en el finally del **try-catch**, para asegurarnos de que la conexion se cierra independientemente de que todo vaya bien o salten excepciones. Realmente y por claridad, hemos hecho una pequeña trampa en el **finally**: No podemos cerrar la conexion tal cual se está haciendo y deberíamos, por un lado, verificar que conexion no es **null** antes de intentar cerrar y por otro lado, **close()** lanza una excepción que debemos capturar. El código real del finally para una cosa tan sencilla como llamar a un **close()** se complica así

```
finally {
   try {
      if (null != conexion) {
         conexion.close();
      }
   } catch (SQLException e) {
      e.printStackTrace();
}
```

En **EjemploDataSource.java** tienes un ejemplo completo usando **BasicDataSource**.


```
package com.chuidiang.ejemplos.datasource_mysql;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.sql.DataSource;

import org.apache.commons.dbcp.BasicDataSource;

/**
 * Ejemplo simple de uso de BasicDataSource.
 * 
 * @author Chuidiang
 */
public class EjemploDataSource {

    /**
     * @param args
     */
    public static void main(String[] args) {
        new EjemploDataSource();
    }

    /** Pool de conexiones */
    private DataSource dataSource;

    /**
     * Inicializa el pool de conexiones BasicDataSource y realiza una insercion
     * y una consulta
     */
    public EjemploDataSource() {
        inicializaDataSource();
        inserta();
        realizaConsulta();
    }

    /**
     * Inicializacion de BasicDataSource
     */
    private void inicializaDataSource() {
        BasicDataSource basicDataSource = new BasicDataSource();

        basicDataSource.setDriverClassName("com.mysql.jdbc.Driver");
        basicDataSource.setUsername("hibernate");
        basicDataSource.setPassword("hibernate");
        basicDataSource.setUrl("jdbc:mysql://localhost/hibernate");

        // Opcional. Sentencia SQL que le puede servir a BasicDataSource
        // para comprobar que la conexion es correcta.
        basicDataSource.setValidationQuery("select 1");

        dataSource = basicDataSource;
    }

    /**
     * Realiza una insercion, pidiendo una conexion al dataSource y cerrandola
     * inmediatamente despues, para liberarla.
     */
    private void inserta() {
        Connection conexion = null;
        try {
            // BasicDataSource nos reserva una conexion y nos la devuelve.
            conexion = dataSource.getConnection();

            // La insercion.
            Statement ps = conexion.createStatement();
            ps
                    .executeUpdate("insert into person values (null,22,'Pedro','Martinez')");

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            liberaConexion(conexion);
        }
    }

    /**
     * Cierra la conexion. Al provenir de BasicDataSource, en realidad no se
     * esta cerrando. La llamada a close() le indica al BasicDataSource que
     * hemos terminado con dicha conexion y que puede asignarsela a otro que la
     * pida.
     * 
     * @param conexion
     */
    private void liberaConexion(Connection conexion) {
        try {
            if (null != conexion) {
                // En realidad no cierra, solo libera la conexion.
                conexion.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    /**
     * Realiza una consulta a la base de datos y muestra los resultados en
     * pantalla.
     */
    private void realizaConsulta() {
        Connection conexion = null;
        try {
            conexion = dataSource.getConnection();
            Statement sentencia = conexion.createStatement();
            ResultSet rs = sentencia.executeQuery("select * from person");

            // La tabla tiene cuatro campos.
            while (rs.next()) {
                System.out.println(rs.getObject("PERSON_ID"));
                System.out.println(rs.getObject("age"));
                System.out.println(rs.getObject("lastname"));
                System.out.println(rs.getObject("firstname"));
                System.out.println("--------------");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // En el finally, para asegurar que se ejecuta, se cierra la
            // conexion.
            liberaConexion(conexion);
        }
    }
}
```

## Configurar el pool desde un fichero de propiedades: **BasicDataSourceFactory**
Si no queremos poner todos los parámetros necesarios para la conexión directamente en código y usando los métodos **set()** mencionados anteriormente, podemos poner dichos parámetros en un fichero de propiedades de java **datasource_config.properties**, similar al siguiente:
```
# Propiedades para la conexion a base de datos con BasicDataSource
driverClassName=com.mysql.jdbc.Driver
username=hibernate
password=hibernate
url=jdbc:mysql://localhost/hibernate
validationQuery=select 1
```
en el que el nombre de cada propiedad se extrae del método set correspondiente, quitando set y pasando la primera letra a minúscula (**BasicDataSource** no es más que un bean de java, con métodos **set()** y **get()** para sus propiedades). Ahora sólo nos queda cargar este fichero de propiedades en nuestro código
```
Properties propiedades = new Properties();
propiedades.load(new FileInputStream(
   "src/main/config/datasource_config.properties"));
```

siendo **java.util.Properties** una clase estándar de java.

Para obtener nuestro **BasicDataSource** a partir de estas propiedades cargadas, debemos usar el método **createDataSource()** de la clase **BasicDataSourceFactory** que viene con **commons-dbcp**. El código para ello puede ser como este
```
DataSource dataSource = BasicDataSourceFactory.createDataSource(propiedades);
```

El resto, cómo realizar una transacción, es igual que el caso anterior. Tienes un ejemplo completo en 
**EjemploBasicDataSourceFactory.java**

```
package com.chuidiang.ejemplos.datasource_mysql;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

import javax.sql.DataSource;

import org.apache.commons.dbcp.BasicDataSourceFactory;

/**
 * Ejemplo simple de uso de BasicDataSourceFactory.
 * 
 * @author Chuidiang
 */
public class EjemploBasicDataSourceFactory {

    /**
     * @param args
     */
    public static void main(String[] args) {
        new EjemploBasicDataSourceFactory();
    }

    /** Pool de conexiones */
    private DataSource dataSource;

    /**
     * Inicializa el pool de conexiones BasicDataSource y realiza una insercion
     * y una consulta
     */
    public EjemploBasicDataSourceFactory() {
        inicializaDataSource();
        inserta();
        realizaConsulta();
    }

    /**
     * Inicializacion de BasicDataSource usando un BasicDataSourceFactory
     */
    private void inicializaDataSource() {
        Properties propiedades = new Properties();
        try {
            propiedades.load(new FileInputStream(
                    "src/main/config/datasource_config.properties"));
            dataSource = BasicDataSourceFactory.createDataSource(propiedades);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * Realiza una insercion, pidiendo una conexion al dataSource y cerrandola
     * inmediatamente despues, para liberarla.
     */
    private void inserta() {
        Connection conexion = null;
        try {
            // BasicDataSource nos reserva una conexion y nos la devuelve.
            conexion = dataSource.getConnection();

            // La insercion.
            Statement ps = conexion.createStatement();
            ps
                    .executeUpdate("insert into person values (null,22,'Pedro','Martinez')");

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            liberaConexion(conexion);
        }
    }

    /**
     * Cierra la conexion. Al provenir de BasicDataSource, en realidad no se
     * esta cerrando. La llamada a close() le indica al BasicDataSource que
     * hemos terminado con dicha conexion y que puede asignarsela a otro que la
     * pida.
     * 
     * @param conexion
     */
    private void liberaConexion(Connection conexion) {
        try {
            if (null != conexion) {
                // En realidad no cierra, solo libera la conexion.
                conexion.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    /**
     * Realiza una consulta a la base de datos y muestra los resultados en
     * pantalla.
     */
    private void realizaConsulta() {
        Connection conexion = null;
        try {
            conexion = dataSource.getConnection();
            Statement sentencia = conexion.createStatement();
            ResultSet rs = sentencia.executeQuery("select * from person");

            // La tabla tiene cuatro campos.
            while (rs.next()) {
                System.out.println(rs.getObject("PERSON_ID"));
                System.out.println(rs.getObject("age"));
                System.out.println(rs.getObject("lastname"));
                System.out.println(rs.getObject("firstname"));
                System.out.println("--------------");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // En el finally, para asegurar que se ejecuta, se cierra la
            // conexion.
            liberaConexion(conexion);
        }
    }
}
```

Ahora puedes pasar a [mysql-java-basico](mysql-java-basico.md). 

## Referencias

[chuidiang](http://www.chuidiang.org/java/mysql/BasicDataSource-Pool-Conexiones.php)