# PRACTICA 4 :  SISTEMAS OPERATIVOS EN TIEMPO REAL  
Alumna: Àfrica Abad

El objetivo de la practica es comprender el funcionamiento de un sistema operativo en tiempo 
Real .

Para lo cual realizaremos una practica  donde  generaremos varias tareas  y veremos como 
se ejecutan dividiendo el tiempo de uso de la cpu.



## Ejercicio Practico 1 


1. Descibir la salida por el puerto serie 


    this is ESP32 task
    this is another task


La salida por el puerto serie intercala estos dos mensajes que se repiten continuamente, ya que ambas funciones se ejecutan en paralelo.


2. Explicar el funcionamiento 
En esta práctica, podemos ver el funcionamiento de dos tareas ejecutándose a la vez en las dos CPUs (Central Processing Unit) de la ESP32.

La función **void setup()**


      void setup() {

      Serial.begin(112500);
      /* we create a new task here */
      xTaskCreate( anotherTask,"another Task", 10000,NULL,1,NULL); 
      }


Primero configuramos la comunicación serial para enviar y recibir datos a través del puerto serie a una velocidad de 112500 baudios. 

Usamos la función **xTaskCreate()** para crar una nueva tarea llamada 'anotherTask'. Esto nos permite ejecutar código en paralelo con la tarea principal. A esta función le asignamos un tamaño de 10000 bytes para almacenar sus variables y datos.  

Cada tarea que creamos puede tener una prioridad assingnada. Las prioridades van de 0 a 24, contra más bajo sea el número, mayor prioridad le damos. En estro caso, asignamos una prioridad de 1 a la tarea **anotherTask**. Si hay múltiples tareas ejecutándose al mismo tiempo, la tarea con la prioridad más alta será la primera en ejecutarse. Si hubiera dos tareas con el mismo grado de prioridad, el tiempo de CPU se repartiria.


La función **loop()** es el bucle principal del programa. Este es el que saca por el puerto serie el mensaje: 
    

    this is ESP32 task


Acto seguido espera 1 segundo antes de repetir el proceso (el tiempo de espera lo podemos modificar con el 'delay()').

La función **anotherTask()** : declara una tarea


    void anotherTask( void * parameter )
    {
    /* loop forever */
    for(;;)
    {
    Serial.println("this is another Task");
    delay(1000);
    }
    /* delete a task when finish,
    this will never happen because this is infinity loop */
    vTaskDelete( NULL );
    }


es un bucle infinito que se ejecuta en paralelo con la función **loop()**. Esta nos imprime por el puerto serie:     


    this is another task


y espera un tiempo (modificable en el 'delay()') de x segundos antes de repetir el proceso.
La función **vTaskDelete(NULL)** en este contexto es inalcanzable y no tiene ningún efecto práctico ya que está dentro de un bucle infinito. 
De todos modos, la función es utilizada en sistemas basados en FreeRTOS (un sistema operativo en tiempo real) para eliminar una tarea cuando ya no es necesaria o cuando ha completado su labor.  Cuando se pasa 'NULL', indica que se desea eliminar la tarea actual que está llamando a 'vTaskDelete()'.



## CONCLUSIÓN

En esta práctica hemos podido ver como el mismo microcontrolador puede correr múltiples tareas simultáneamente, con operaciones independientes en paralelo.




