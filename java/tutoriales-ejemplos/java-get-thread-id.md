# Obtener Thread Id en Java
## Contenido
1. [Obtener el ID del hilo usando Thread.getId() en Java](java-get-thread-id.md#obtener-el-id-del-hilo-usando-threadgetid-en-java)
2. [Obtener el id de la piscina de hilos actual usando Thread.currentThread().getId() en Java](java-get-thread-id.md#obtener-el-id-de-la-piscina-de-hilos-actual-usando-threadcurrentthreadgetid-en-java)

En este tutorial, introduciremos métodos para obtener el ID del hilo en Java. También veremos cómo podemos obtener el id del hilo actual de un grupo de hilos.

## Obtener el ID del hilo usando Thread.getId() en Java

En este ejemplo, hemos creado una clase Task que implementa la clase Runnable porque necesitamos su método run() para ejecutar el hilo. La clase Task toma el nombre del hilo de su constructor, y el método run() lo imprime en la consola cuando se ejecuta.

En el método main(), creamos dos objetos Task en el constructor y luego dos objetos threads en los que pasamos task1 y task2 para asignar las tareas.

Llamaremos al método start() usando thread1 y thread2 para ejecutar los hilos. Por último, una vez que los hilos han sido ejecutados, podemos obtener el id de cada hilo usando thread.getId(), que devuelve el id como long.

```
public class GetThreadID {
    public static void main(String[] args) {

        Task task1 = new Task("Task 1");
        Task task2 = new Task("Task 2");
      
        Thread thread1 = new Thread(task1);
        Thread thread2 = new Thread(task2);

        thread1.start();
        thread2.start();

        System.out.println("Thread1's ID is: " + thread1.getId());
        System.out.println("Thread2's ID is: " + thread2.getId());

    }
}

class Task implements Runnable {

    private String name;

    Task(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println("Executing " + name);
    }
}
```

Producción:
```
Thread1's ID is: 13
Thread2's ID is: 14
Executing Task 2
Executing Task 1
```

## Obtener el id de la piscina de hilos actual usando Thread.currentThread().getId() en Java

Los charcos de hilos son beneficiosos cuando se trata de la ejecución de tareas pesadas. En el ejemplo de abajo, creamos un pool de hilos usando Executors.newFixedThreadPool(numberOfThreads). Podemos especificar el número de hilos que queremos en el pool.

La clase Task es la responsable de ejecutar el hilo en el método run(). Es una clase simple que establece y obtiene el nombre del hilo pasado en el constructor. Para crear múltiples tareas, usamos un bucle for en el que se crean cinco objetos task y se ejecutan cinco hilos en el pool.

Nuestro objetivo es obtener el id de cada hilo que se está ejecutando actualmente. Para ello, usaremos Thread.currentThread().getId() que devuelve el id del hilo actual. En la salida, podemos ver los ids de todos los hilos que ejecutan las tareas individuales.

Una vez que las tareas se completan, debemos dejar de ejecutar el conjunto de hilos usando threadExecutor.shutdown(). !threadExecutor.isTerminated() se usa para esperar hasta que el threadExecutor haya terminado.

```
package com.company;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class GetThreadID {
    public static void main(String[] args) {
        
        int numberOfThreads = 5;

        ExecutorService threadExecutor = Executors.newFixedThreadPool(numberOfThreads);

        for (int i = 0; i < 5; i++) {
            Task task = new Task("Task " + i);

            System.out.println("Created Task: " + task.getName());
            threadExecutor.execute(task);
        }
        threadExecutor.shutdown();
      
        while (!threadExecutor.isTerminated()) {
        }

        System.out.println("All threads have completed their tasks");

    }
}

class Task implements Runnable {

    private String name;

    Task(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public void run() {

        System.out.println("Executing: " + name);
        System.out.println(name + " is on thread id #" + Thread.currentThread().getId());

    }
}
```

Producción:
```
Created Task: Task 0
Created Task: Task 1
Created Task: Task 2
Created Task: Task 3
Created Task: Task 4
Executing: Task 0
Executing: Task 2
Executing: Task 1
Executing: Task 4
Executing: Task 3
Task 0 is on thread id #13
Task 1 is on thread id #14
Task 4 is on thread id #17
Task 2 is on thread id #15
Task 3 is on thread id #16
All threads have completed their tasks
```

## Relacionado
- [Hilo de demonio en Java](daemon-thread-java.md)  
- [Mata Thread en Java](kill-thread-java.md)


## Referencias
- [delftstack](https://www.delftstack.com/es/howto/java/daemon-thread-java/)