
# Mata Thread en Java
1. [Matar o detener un hilo usando una booleana indicador en Java](kill-thread-java.md#matar-o-detener-un-hilo-usando-una-booleana-indicador-en-java)
2. [Matar o detener un hilo usando interrupt() en Java](kill-thread-java.md#matar-o-detener-un-hilo-usando-interrupt-en-java)

Los hilos en Java nos permiten ejecutar varias tareas en paralelo, lo que permite la multitarea. Podemos crear un hilo en Java usando la clase Thread. En este artículo, introduciremos dos métodos para matar un hilo.

Aunque el hilo es destruido por el método run() de la clase Thread una vez que ha completado todas las tareas, a veces podemos querer matar o detener un hilo antes de que haya terminado de ejecutarse completamente.

## Matar o detener un hilo usando una booleana indicador en Java
Para matar explícitamente un hilo, podemos usar una bandera booleana para notificar al hilo cuándo detener la tarea. Debajo, dos hilos imprimen una línea con su nombre, y luego ambos hilos duermen durante unos 100 milisegundos. Los hilos se ejecutan hasta que la bandera booleana exitThread se hace realidad.

thread1 y thread2 son dos hilos creados, y se pasa un nombre como argumento en ambos. Como el constructor de la clase ExampleThread tiene thread.start() que inicia la ejecución de un hilo, ambos hilos se ejecutarán. Podemos ver que la salida imprime los nombres de ambos hilos de forma aleatoria porque se están ejecutando en paralelo.

Para detener el hilo, llamaremos a stopThread(), que es un método en ExampleThread que establece exitThread como true, y el hilo finalmente se detiene porque while(!exitThread) se convierte en false.

```
public class KillThread {
    public static void main(String[] args) {

        ExampleThread thread1 = new ExampleThread("Thread One");
        ExampleThread thread2 = new ExampleThread("Thread Two");

        try {
            Thread.sleep(500);
            thread1.stopThread();
            thread2.stopThread();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("**Exiting the main Thread**");

    }
}

class ExampleThread implements Runnable {

    private String name;
    private boolean exitThread;
    Thread thread;

    ExampleThread(String name) {
        this.name = name;
        thread = new Thread(this, name);
        System.out.println("Created Thread: " + thread);
        exitThread = false;
        thread.start();
    }

    @Override
    public void run() {

        while (!exitThread) {
            System.out.println(name + " is running");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println(name + " has been Stopped.");

    }

    public void stopThread() {
        exitThread = true;
    }
}
```

Producción:
```
Created Thread: Thread[Thread One,5,main]
Created Thread: Thread[Thread Two,5,main]
Thread Two is running
Thread One is running
Thread Two is running
Thread One is running
Thread One is running
Thread Two is running
Thread One is running
Thread Two is running
Thread One is running
Thread Two is running
**Exiting the main Thread**
Thread Two has been Stopped.
Thread One has been Stopped.
```

## Matar o detener un hilo usando interrupt() en Java

Usaremos el ejemplo anterior aquí, pero con un nuevo método llamado interrupt(). Esta función detiene la ejecución inmediatamente cuando se llama a un hilo. En este ejemplo, estamos usando thread.isInterrupted() para comprobar si se ha llamado a interrupt().

Para detener ambos hilos, llamaremos a thread1.thread.interrupt() y thread2.thread.interrupt() lo que resultará en la terminación de los hilos.

```
public class KillThread {
    public static void main(String[] args) {

        ExampleThread thread1 = new ExampleThread("Thread One");
        ExampleThread thread2 = new ExampleThread("Thread Two");

        try {
            Thread.sleep(6);
            thread1.thread.interrupt();
            thread2.thread.interrupt();
            Thread.sleep(8);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }


        System.out.println("**Exiting the main Thread**");

    }
}


class ExampleThread implements Runnable {
    private String name;
    Thread thread;

    ExampleThread(String name) {
        this.name = name;

        thread = new Thread(this, name);
        System.out.println("Created Thread: " + thread);

        thread.start();
    }

    @Override
    public void run() {

        while (!thread.isInterrupted()) {
            System.out.println(name + " is running");
        }

        System.out.println(name + " has been Stopped.");

    }
}
```

Producción:
```
Created Thread: Thread[Thread One,5,main]
Created Thread: Thread[Thread Two,5,main]
Thread One is running
Thread Two is running
Thread One has been Stopped.
Thread Two has been Stopped.
**Exiting the main Thread**
```

## Relacionado
- [Hilo de demonio en Java](daemon-thread-java.md)
- [Obtener Thread Id en Java](java-get-thread-id.md)


## Referencias
- [delftstack](https://www.delftstack.com/es/howto/java/daemon-thread-java/)