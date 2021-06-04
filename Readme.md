#PRÁCTICA 4 : Sistemas operativos en tiempo real

##Código

```
#include <Arduino.h>
void anotherTask( void * parameter );
void setup()
{
Serial.begin(112500);
/* we create a new task here */
xTaskCreate(
anotherTask, /* Task function. */
"another Task", /* name of task. */
10000, /* Stack size of task */
NULL, /* parameter of the task */
1, /* priority of the task */
NULL); /* Task handle to keep track of created task */
}
/* the forever loop() function is invoked by Arduino ESP32 loopTask */
void loop()
{
Serial.println("this is ESP32 Task");
delay(1000);
}
/* this function will be invoked when additionalTask was created */
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
```

#Salida por el puerto serie

En el puerto serie sale algo asi:

this is ESP32 Task
this is another Task
this is ESP32 Task
this is another Task
this is ESP32 Task
this is another Task
this is ESP32 Task
this is another Task
this is ESP32 Task
this is another Task
this is ESP32 Task
this is another Task

Esto es lo que saldria.

#Funcionamiento

Primero en el void setup() inicializaremos una comunicación en serie con una velocidad de 115200 bauds. Seguidamente informaremos al planificador sobre nuestra tarea con `n xTaskCreate` donde definiremos la funcion task, el nombre, el tamaño del stack, el parametro y su prioridad.

```
void setup()
{
Serial.begin(112500);
xTaskCreate(
anotherTask,
"another Task",
10000,
NULL,
1,
NULL);
}
```

En el void loop() crearemos un bucle infinito para mostrar por el terminal (this is
ESP32 Task).

```
void loop()
{
Serial.println("this is ESP32 Task");
delay(1000);
}
```

Por ultimo llamaremos a la función `void anothertask( void * parameter)` donde haremos un bucle infinito mostrando en el terminal (this is another Task) y despues con `vTaskDelate( NULL )` eliminaremos la tarea cuando finalice, pero es no sucedera por que es un bucle infinito.

```
void anotherTask( void * parameter )
{
for(;;)
{
Serial.println("this is another Task");
delay(1000);
}
vTaskDelete( NULL );
}
```

