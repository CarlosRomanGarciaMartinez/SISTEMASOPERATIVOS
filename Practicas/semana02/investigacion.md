# Tecnológico Nacional de México (TecNM) Campus Rioverde
**Ingeniería en Sistemas Computacionales**
**Materia:** Sistemas Operativos
**Alumno:** Carlos Román García Martínez
**Práctica 02:** Procesos y multiprogramación en Ubuntu

---

## 📖 Investigación Teórica

### 1. Conceptos Básicos
*   **¿Qué es un proceso?**
    *   Es una instancia de un programa en ejecución dentro del sistema operativo. Cuenta con su propio espacio de memoria virtual, un identificador único (PID), un contexto de ejecución y recursos asignados por el kernel.
*   **¿Qué diferencia existe entre programa y proceso?**
    *   Un programa es una entidad estática, un conjunto de instrucciones almacenadas en un archivo ejecutable en el disco duro. Un proceso es una entidad dinámica y activa, es decir, ese mismo programa cargado en la memoria principal y siendo ejecutado por el procesador.
*   **¿Por qué un mismo programa puede generar varios procesos?**
    *   Porque el sistema operativo permite que múltiples instancias de un mismo archivo ejecutable corran de manera independiente en la memoria. Cada instancia obtiene su propio PID, espacio de datos y recursos aislados del resto.
*   **¿Qué es un PID?**
    *   *Process ID* (Identificador de Proceso). Es un número entero único que el sistema operativo asigna de forma secuencial a cada proceso al momento de su creación para poder administrarlo y distinguirlo de los demás.
*   **¿Qué es un PPID?**
    *   *Parent Process ID* (Identificador del Proceso Padre). Es el PID correspondiente al proceso que creó o invocó al proceso actual, estableciendo así una relación jerárquica.

### 2. Estados de un Proceso
*   **¿Qué significan los estados que aparecen en la columna `STAT` (por ejemplo, S, R, Z, etc.)?**
    *   Representan la condición actual en la que se encuentra el proceso ante el planificador del kernel:
        *   **R** (*Running*): Ejecutándose o listo para ser ejecutado en la CPU.
        *   **S** (*Sleeping*): En suspensión interrumpible, esperando a que ocurra un evento o recurso.
        *   **D** (*Disk Sleep*): En suspensión ininterrumpida, generalmente esperando operaciones de E/S de disco.
        *   **Z** (*Zombie*): Proceso finalizado cuyo padre aún no ha leído su estado de salida.
        *   **T** (*Stopped*): Detenido o pausado por una señal de control.
*   **¿Por qué un proceso no permanece necesariamente todo el tiempo utilizando la CPU?**
    *   Porque el tiempo de procesamiento se reparte por turnos (multiprogramación) y porque muchos procesos pasan la mayor parte del tiempo esperando eventos externos, entradas de usuario o respuestas de E/S (como discos o redes), entrando en estados de suspensión.

### 3. Ejecución y Trabajos
*   **¿Qué es un proceso en primer plano (foreground)?**
    *   Es aquel que interactúa directamente con la terminal, tomando el control de la entrada estándar y bloqueando la línea de comandos hasta que finalice su ejecución.
*   **¿Qué es un proceso en segundo plano (background)?**
    *   Es un proceso que se ejecuta de manera independiente sin bloquear la terminal, permitiendo que el usuario siga introduciendo comandos de forma simultánea.
*   **¿Qué es un Job en la terminal?**
    *   Es una tarea o proceso gestionado directamente por la capa de control de trabajos (*job control*) de la shell actual, al cual se le asigna un identificador numérico de trabajo (ej. `[1]`).

### 4. Prioridad de Procesos
*   **¿Qué significan `NI` y `NICE`?**
    *   `NI` (*Nice value*) es el valor numérico de gentileza o prioridad asignado a un proceso, el cual oscila comúnmente entre -20 (máxima prioridad) y 19 (mínima prioridad). `nice` es el comando utilizado para modificar este parámetro al iniciar un programa.
*   **¿Por qué un sistema operativo necesita mecanismos para administrar la prioridad de los procesos?**
    *   Para garantizar que las tareas críticas del sistema o del usuario reciban más tiempo de CPU frente a procesos secundarios o de fondo, optimizando el rendimiento general del equipo.
*   **¿Cambiar la prioridad significa que el proceso obtiene una CPU exclusiva?**
    *   No. Modificar el valor de `nice` únicamente ajusta qué tan "gentil" es el proceso al ceder ciclos de reloj; no le otorga exclusividad sobre el procesador, sino una mayor o menor preferencia en la cola de planificación del kernel.

### 5. El directorio `/proc`
*   **¿Qué es el directorio `/proc` en Linux?**
    *   Es un sistema de archivos virtual creado en memoria por el kernel de Linux. No ocupa espacio físico en el disco duro y sirve como una interfaz de comunicación para consultar y modificar parámetros del sistema y de los procesos en tiempo real.
*   **¿Qué relación existe entre los directorios numéricos dentro de `/proc` y los PID?**
    *   Cada directorio cuyo nombre es un número dentro de `/proc` corresponde exactamente al PID de un proceso activo en el sistema, conteniendo archivos con información detallada de ese proceso específico.
*   **¿Qué ventaja tiene `/proc` para un administrador de sistemas?**
    *   Permite inspeccionar de forma directa, rápida y sin herramientas complejas el estado interno de los procesos y del hardware mediante la simple lectura de archivos de texto plano.
