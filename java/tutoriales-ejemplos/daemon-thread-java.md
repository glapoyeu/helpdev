# Hilo de demonio en Java

## Contenido
1. [Crear hilo de demonio usando el método **setDaemon()** en Java](daemon-thread-java.md#crear-hilo-de-demonio-usando-el-método-setdaemon-en-java)
2. [Cree un subproceso de demonio utilizando el método setDaemon() en Java](daemon-thread-java.md#cree-un-subproceso-de-demonio-utilizando-el-método-setdaemon-en-java)
3. [Cree un subproceso de demonio utilizando el método setDaemon() en Java](daemon-thread-java.md#cree-un-subproceso-de-demonio-utilizando-el-método-setdaemon-en-java)

Este tutorial presentará qué es un subproceso de demonio y cómo crear un subproceso de demonio en Java.

En Java, un subproceso de demonio es un subproceso especial que admite subprocesos en segundo plano para otros subprocesos. Java utiliza estos subprocesos para fines especiales de subprocesos de usuario y recolección de basura, etc.

Para crear un hilo de demonio, Java proporciona el método **setDaemon()** que toma un argumento booleano. El método **isDaemon()** puede comprobar si el hilo actual en ejecución es un hilo Daemon o no.

## Crear hilo de demonio usando el método **setDaemon()** en Java

En este ejemplo, usamos el método **setDaemon()** de la clase **Thread** para crear un subproceso demonio. Creamos tres subprocesos, y dos de ellos se establecen como subprocesos de demonio. Usamos el método **isDaemon()** para comprobar si el hilo en ejecución actual es un hilo demonio o no. Vea el ejemplo a continuación.

```
public class SimpleTesting extends Thread{ 
    public MainClass(String name){ 
        super(name); 
    }
    public void run() 
    {   
        boolean isDaemon = Thread.currentThread().isDaemon();
        System.out.println("Is this daemon thread: "+isDaemon);
    }  
    public static void main(String[] args) {
        MainClass thread1 = new MainClass("Thread 1"); 
        MainClass thread2 = new MainClass("Thread 2"); 
        MainClass thread3 = new MainClass("Thread 3"); 
        thread1.setDaemon(true); 
        thread1.start();  
        thread2.start(); 
        thread3.setDaemon(true);  
        thread3.start();         
    }  
}
```

Producción:
```
Is this daemon thread: true
Is this daemon thread: false
Is this daemon thread: true
```

## Cree un subproceso de demonio utilizando el método **setDaemon()** en Java
El subproceso daemon es el subproceso de baja prioridad y siempre se ejecuta detrás de los subprocesos del usuario. Podemos ver la prioridad de un hilo usando el método **getPriority()** que devuelve un valor entero. Usamos el método **getName()** para buscar el nombre del hilo en ejecución actual. Vea el siguiente ejemplo.
```
public class SimpleTesting extends Thread{ 
    public MainClass(String name){ 
        super(name); 
    }
    public void run() 
    {   
        boolean isDaemon = Thread.currentThread().isDaemon();
        System.out.println("Is this daemon thread: "+isDaemon+", "+Thread.currentThread().getName()+", "+Thread.currentThread().getPriority());
    }  
    public static void main(String[] args) {
        MainClass thread1 = new MainClass("Thread 1"); 
        MainClass thread2 = new MainClass("Thread 2"); 
        MainClass thread3 = new MainClass("Thread 3"); 
        thread1.setDaemon(true); 
        thread1.start();  
        thread2.start(); 
        thread3.setDaemon(true);  
        thread3.start();         
    }  
}
```
Producción:
```
Is this daemon thread: true, Thread 1, 5
Is this daemon thread: false, Thread 2, 5
Is this daemon thread: true, Thread 3, 5
```

## Cree un subproceso de demonio utilizando el método **setDaemon()** en Java
Comenzamos el hilo después de establecer un valor para el método **setDaemon()** mientras creamos un hilo demonio. Es esencial porque si configuramos un hilo que ya se está ejecutando como demonio, arrojará una excepción. Llamamos al método **start()** con **thread1** y luego llamamos a **setDaemon()**, que no está permitido. Vea el siguiente ejemplo.
```
public class SimpleTesting extends Thread{ 
    public MainClass(String name){ 
        super(name); 
    }
    public void run() 
    {   
        boolean isDaemon = Thread.currentThread().isDaemon();
        System.out.println("Is this daemon thread: "+isDaemon+", "+Thread.currentThread().getName()+", "+Thread.currentThread().getPriority());
    }  
    public static void main(String[] args) {
        MainClass thread1 = new MainClass("Thread 1"); 
        MainClass thread2 = new MainClass("Thread 2"); 
        MainClass thread3 = new MainClass("Thread 3");  
        thread1.start();  
        thread1.setDaemon(true);
        thread2.start(); 
        thread3.setDaemon(true);  
        thread3.start();         
    }  
}
```

Producción:
```
Is this daemon thread: false, Thread 1, 5
Exception in thread "main" java.lang.IllegalThreadStateException
```

## Relacionado
- [Mata Thread en Java](kill-thread-java.md)
- [Obtener Thread Id en Java](java-get-thread-id.md)

## Referencias
- [delftstack](https://www.delftstack.com/es/howto/java/daemon-thread-java/)











