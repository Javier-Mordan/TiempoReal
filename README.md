# TiempoReal

Monitoreo de Señales de Drones en Tiempo Real
El proyecto tiene como objetivo desarrollar un sistema en tiempo real para monitorear las señales provenientes de drones, procesarlas y mostrar alertas cuando se superen límites específicos, como la altitud máxima permitida. Además, el sistema permite visualizar en una interfaz gráfica los datos de las señales en tiempo real, como la altitud y velocidad del dron. Utilizando conceptos de Programación en Tiempo Real (PTR), se implementan semáforos para asegurar la sincronización de hilos y evitar problemas como las condiciones de carrera en el acceso concurrente a la base de datos.
Diseños y Arquitecturas:
Base de Datos: La base de datos está diseñada con tres tablas principales las cuales se detallarán a continuación:
drone_signals: Esta tabla almacena las señales del dron, como el timestamp, la altitud y la velocidad.
Campos:
id: Identificador único de la señal.
timestamp: Fecha y hora en que se registró la señal.
altitude: Altitud en metros del dron.
speed: Velocidad en metros por segundo del dron.
processed: Un valor que indica si la señal ha sido procesada.
control_signals: Guarda información relacionada con los valores de control enviados a los drones y posibles correcciones aplicadas a los mismos.
Campos:
id: Identificador único del valor de control.
timestamp: Fecha y hora en que se generó el valor de control.
control_value: El valor de control recibido.
correction: Si aplica, una corrección que se realiza al valor de control.
errors: Esta tabla almacena los errores que pueden ocurrir durante el procesamiento de las señales.
Campos:
id: Identificador único del error.
timestamp: Fecha y hora del error.
error_message: Descripción del error.

Las relaciones entre estas tablas permiten la captura, el procesamiento y el registro de alertas de manera estructurada.
Arquitectura de la Aplicación: El sistema está dividido en varias capas:
Captura de señales: Un hilo independiente se encarga de simular la captura de señales del dron. Este hilo genera aleatoriamente las altitudes y velocidades de los drones y las guarda en la base de datos. El hilo se ejecuta indefinidamente con un intervalo de 1 segundo.
Procesamiento de señales: Otro hilo se encarga de procesar las señales almacenadas en la base de datos. Este hilo realiza ajustes en los valores de altitud y velocidad de las señales y marca los registros como procesados. También maneja los errores durante el procesamiento y los registra en la tabla de errores.
Interfaz gráfica (UI): La interfaz gráfica, creada con Tkinter, permite visualizar las señales capturadas y procesadas. Se actualiza cada 2 segundos para reflejar los datos más recientes. La tabla muestra la marca de tiempo, altitud y velocidad de las últimas señales.
Sincronización: Para evitar condiciones de carrera y problemas de acceso concurrente a la base de datos, se utiliza un semáforo en todos los puntos donde se accede o modifica la base de datos. Esto asegura que solo un hilo tenga acceso a la base de datos a la vez.

Problemas a Resolver:
Sincronización de Acceso a la Base de Datos: El principal desafío en este proyecto es garantizar que varios hilos puedan acceder y modificar la base de datos sin interferencias. Esto se resuelve mediante el uso de semáforos, que, gracias a nuestro diseño, aseguran que solo un hilo pueda acceder a la base de datos en un momento dado. Esta técnica es crucial para evitar inconsistencias en los datos y garantizar la integridad del sistema.
El semáforo (semaphore) se utiliza en las funciones que interactúan con la base de datos, como la inserción de señales (capture_drone_signal), la actualización de las señales procesadas (process_drone_signal) y el registro de errores (log_error). Esto garantiza que cuando un hilo esté escribiendo o leyendo datos de la base de datos, otros hilos no puedan interferir, manteniendo la coherencia de la información.
Alertas de Altitud: En el monitoreo de drones, es importante asegurarse de que no excedan los límites de altitud. En este proyecto, si la altitud de una señal supera los 80 metros, el sistema genera una alerta visual utilizando el messagebox de Tkinter y registra el evento en la base de datos. Esto permite al usuario estar informado sobre posibles violaciones de las normativas de vuelo.
Procesamiento de Datos: El sistema también ajusta las señales en función de ciertos factores (por ejemplo, aumentando la altitud y la velocidad en un 5% y 2%, respectivamente). Estas modificaciones permiten simular un ajuste de los datos antes de marcarlos como procesados. Los errores durante este proceso se registran en la base de datos para un análisis posterior.

Implementación de Semáforos: La sincronización de los hilos es un aspecto clave en la programación en tiempo real. En este proyecto, se ha utilizado un semaforo para controlar el acceso concurrente a la base de datos SQLite. Dado que tanto el hilo que captura las señales como el que las procesa necesitan acceder a la base de datos, se usa el semáforo para asegurar que solo uno de estos hilos pueda interactuar con la base de datos a la vez.
La implementación es sencilla pero efectiva. Cada vez que un hilo necesita acceder a la base de datos, primero adquiere el semáforo (semaphore.acquire()) y, al finalizar, libera el semáforo (semaphore.release()). Esto garantiza que no haya problemas de acceso simultáneo, lo que podría causar errores o corrupción de datos.
Conclusión
Este proyecto ha permitido aplicar los conceptos fundamentales de Programación en Tiempo Real, especialmente en cuanto a la sincronización de hilos y el manejo de bases de datos concurrentes. A través del uso de semáforos, se ha logrado evitar las condiciones de carrera y asegurar que las señales del dron se procesen de manera correcta y sin interferencias. Además, se ha implementado un sistema de alertas que permite notificar al usuario cuando el dron excede la altitud máxima permitida.
En cuanto a la interfaz gráfica, se ha desarrollado una solución eficiente y clara para mostrar los datos en tiempo real, lo que permite al usuario monitorear el sistema de manera efectiva. A pesar de ser un sistema sencillo, demuestra cómo se pueden integrar múltiples conceptos de programación en tiempo real en una aplicación práctica.

