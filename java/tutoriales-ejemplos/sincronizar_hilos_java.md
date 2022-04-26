Sincronización de hilos

... viene de hilos en java.

Cuando en un programa tenemos varios hilos corriendo simultáneamente es posible que varios hilos intenten acceder a la vez a un mismo sitio (un fichero, una conexión, un array de datos) y es posbible que la operación de uno de ellos entorpezca la del otro. Para evitar estos problemas, hay que sincronizar los hilos. Por ejemplo, si un hilo con vocación de Cervantes escribe en fichero "El Quijote" y el otro con vocación de Shakespeare escribe "Hamlet", al final quedarán todas las letras entremezcladas. Hay que conseguir que uno escriba primero su Quijote y el otro, luego, su Hamlet.
Sincronizar usando un objeto

Imagina que escribimos en un fichero usando una variable fichero de tipo PrintWriter. Para escribir uno de los hilos hará esto

fichero.println("En un lugar de la Mancha...");

Mientras que el otro hará esto

fichero.println("... ser o no ser ...");

Si los dos hilos lo hacen a la vez, sin ningún tipo de sincronización, el fichero al final puede tener esto

En un ... ser lugar de la o no Mancha ser ...

Para evitarlo debemos sincronizar los hilos. Cuando un hilo escribe en el fichero, debe marcar de alguna manera que el fichero está ocupado. El otro hilo, al intentar escribir, lo verá ocupado y deberá esperar a que esté libre. En java esto se hace fácilmente. El código sería así

synchronized (fichero)
{
   fichero.println("En un lugar de la Mancha...");
}

y el otro hilo

synchronized (fichero)
{
   fichero.println("... ser o no ser ...");
}

Al poner synchronized(fichero) marcamos fichero como ocupado desde que se abren las llaves de después hasta que se cierran. Cuando el segundo hilo intenta también su synchronized(fichero), se queda ahí bloqueado, en espera que de que el primero termine con fichero. Es decir, nuestro hilo Shakespeare se queda parado esperando en el synchronized(fichero) hasta que nuestro hilo Cervantes termine.

synchronized comprueba si fichero está o no ocupado. Si está ocupado, se queda esperando hasta que esté libre. Si está libre o una vez que esté libre, lo marca como ocupado y sigue el código.

Este mecanismo requiere colaboración entre los hilos. El que hace el código debe acordarse de poner synchronized siempre que vaya a usar fichero. Si no lo hace, el mecanismo no sirve de nada.
Métodos sincronizados

Otro mecanismo que ofrece java para sincronizar hilos es usar métodos sincronizados. Este mecanismo evita además que el que hace el código tenga que acordarse de poner synchronized.

Imagina que encapsulamos fichero dentro de una clase y que ponemos un método synchronized para escribir, tal que así

public class ControladorDelFichero
{
   private PrintWriter fichero;

   public ControladorFichero()
   {
      // Aqui abrimos el fichero y lo dejamos listo
      // para escribir.
   }

   public synchronized void println(String cadena)
   {
       fichero.println(cadena);
   }
}

Una vez hecho esto, nuestros hilos Cervantes y Shakespeare sólo tienen que hacer esto

ControladorFichero control = new ControladorFichero();
...
// Hilo Cervantes
control.println("En un lugar de la Mancha ...");
...
// Hilo Shakespeare
control.println("... ser o no ser ...");

Al ser el método println() synchronized, si algún hilo está dentro de él ejecutando el código, cualquier otro hilo que llame a ese método se quedará bloqueado en espera de que el primero termine.

Este mecanismo es más mejor porque, siguiendo la filosfía de la orientación a objetos, encapsula más las cosas. El fichero requiere sincronización, pero ese conocimiento sólo lo tiene la clase ControladorFichero. Los hilos Cervantes y Shakespeare no saben nada del tema y simplemente se ocupan de escribir cuando les viene bien. Tampoco depende de la buena memoria del programador a la hora de poner el synchronized(fichero) de antes.
Otros objetos que necesitan sincronización

Hemos puesto de ejemplo un fichero, pero requieren sincronización en general cualquier entrada y salida de datos, como pueden ser ficheros, sockets o incluso conexiones con bases de datos.

También pueden necesitar sincronización almacenes de datos en memoria, como LinkedList, ArrayList, etc. Imagina, por ejemplo, en una LinkedList que un hilo está intentando sacar por pantalla todos los datos

LinkedList lista = new LinkedList();
...
for (int i=0;i<lista.size(); i++)
   System.out.println(lista.get(i));

Estupendo y maravilloso pero ... ¿qué pasa si mientras se escriben estos datos otro hilo borra uno de los elementos?. Imagina que lista.size() nos ha devuelto 3 y justo antes de intentar escribir el elemento 2 (el último) viene otro hilo y borra cualquiera de los elementos de la lista. Cuando intentemos el lista.get(2) nos saltará una excepción porque la lista ya no tiene tantos elementos.

La solución es sincronizar la lista mientras la estamos usando

LinkedList lista = new LinkedList();
...
synchronized (lista)
{
   for (int i=0;i<lista.size(); i++)
      System.out.println(lista.get(i));
}

además, este tipo de sincronización es la que se requiere para mantener "ocupado" el objeto lista mientras hacemos varias llamadas a sus métodos (size() y get()), no queda más remedio que hacerla así. Por supuesto, el que borra también debe preocuparse del synchronized.

continuamos bloqueando hilos...