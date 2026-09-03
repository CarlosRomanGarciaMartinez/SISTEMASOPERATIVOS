# Semana 02 - Sistemas Operativos

## 🎯 Objetivo

¿Qué aprendí durante la práctica?
Aprendí a observar, identificar y administrar procesos en un entorno de Ubuntu utilizando la terminal, comprendiendo cómo el sistema operativo transforma un programa en ejecución, cómo administra sus recursos (CPU y memoria) y la importancia fundamental de la multiprogramación y la seguridad al manipular PIDs.

## 🐧 Entorno

¿Qué distribución y versión de Ubuntu utilicé?
Utilicé Ubuntu 24.04 LTS.

¿Qué terminal utilicé?
Utilicé la terminal predeterminada de Ubuntu (Bash).

![Entorno y Usuario](./evidencias/01_entorno_usuario.png)

## 🆔 Procesos

¿Qué es un proceso?
Es un programa en ejecución que cuenta con sus propios recursos asignados, un espacio de memoria y un identificador único (PID) gestionado por el sistema operativo.

¿Qué diferencia existe entre programa y proceso?
Un programa es un archivo estático almacenado en el disco, mientras que un proceso es una entidad dinámica y activa en ejecución en la memoria principal bajo el control del sistema operativo.

## 📊 Diagnóstico

¿Qué información obtuve mediante `ps`?
Obtuve una lista básica de los procesos activos asociados directamente a mi sesión de terminal actual.

¿Qué información obtuve mediante `ps aux`?
Obtuve una fotografía global y detallada de todos los procesos del sistema ejecutados por cualquier usuario, incluyendo columnas clave como CPU, memoria y estado.

![Diagnóstico ps aux](./evidencias/03_comando_ps_aux.png)

## 🌳 Relación padre-hijo

¿Qué proceso padre identifiqué?
Identifiqué a la shell (`bash`) como el proceso padre encargado de gestionar la sesión y los comandos que ejecuto.

¿Qué procesos hijos encontré?
Encontré procesos derivados como comandos del sistema y subprocesos generados desde la terminal.

![Árbol de procesos](./evidencias/04_arbol_pstree.png)

## 📈 Monitoreo

¿Qué observé mediante `top`?
Observé de forma dinámica y en tiempo real el comportamiento del sistema, viendo cómo fluctuaba el consumo de recursos segundo a segundo.

![Monitoreo top](./evidencias/05_monitoreo_top.png)

## 🧪 Procesos de laboratorio

¿Qué procesos creé?
Creé un proceso controlado utilizando el comando de espera `sleep 300`.

¿Qué PID tuvieron?
Tuvieron un identificador numérico asignado dinámicamente por el kernel.

¿Cómo los identifiqué?
Los localicé filtrando la tabla de procesos mediante el comando `ps aux | grep sleep`.

![Proceso de laboratorio](./evidencias/06_proceso_lab_sleep.png)

## 🛠️ Administración

¿Cómo finalicé los procesos de laboratorio?
Utilicé el comando `kill [PID]` utilizando estrictamente el número de PID del proceso que yo mismo había creado.

¿Cómo comprobé que terminaron?
Volví a ejecutar la búsqueda con `ps aux | grep sleep` para verificar que la línea del proceso ya no aparecía activa.

## ⌨️ Primer plano y segundo plano

¿Qué diferencia observé?
Un proceso en primer plano bloquea la terminal impidiendo introducir nuevos comandos, mientras que un proceso en segundo plano se ejecuta de forma independiente liberando la línea de comandos de inmediato.

¿Qué comandos utilicé?
Utilicé el operador `&` para enviarlos al fondo, `jobs` para listarlos, y los comandos `fg` / `bg` para administrarlos.

![Segundo plano y jobs](./evidencias/07_segundo_plano_jobs.png)

## ⚙️ Prioridad

¿Qué investigué sobre `nice`?
Investigé que permite iniciar un proceso modificando su prioridad de planificación (`NI`), donde un número positivo reduce su prioridad y uno negativo la aumenta.

¿Qué resultado obtuve?
Comprobé que al lanzar un proceso con `nice`, la columna `NI` reflejó el valor asignado al ser consultado con `ps`.

![Prioridad nice](./evidencias/08_prioridad_nice.png)

## 🔬 /proc

¿Qué información encontré?
Encontré que Linux expone la información interna del kernel y de cada proceso en tiempo real mediante un sistema de archivos virtual, donde cada directorio numérico corresponde al PID de un proceso.

![Directorio proc](./evidencias/09_directorio_proc.png)

## 🔄 Multiprogramación

¿Qué observé al tener varios procesos ejecutándose?
Observé que múltiples instancias del comando `sleep` coexistieron y avanzaron de forma simultánea, demostrando que el sistema operativo reparte los recursos de manera alternada.

![Multiprogramación](./evidencias/10_multiprogramacion_varios.png)

## 🧠 Reto: diagnóstico

¿Qué problema simulé?
Simulé una situación donde el sistema experimenta un alto consumo de recursos debido a procesos de alta carga en el procesador.

¿Qué proceso investigué?
Investigé los procesos con mayor carga en la CPU utilizando filtros de ordenamiento en `ps aux`.

¿Qué información encontré?
Utilicé el PID, el porcentaje de consumo de recursos y la ruta del comando para determinar si era un proceso seguro de administrar.

## 📊 Comparación de herramientas

*[Nota: Ver tabla comparativa completa en el archivo procesos.md]*

## 🧪 Antes y después

Explica qué ocurrió durante el experimento.
Antes del experimento el sistema tenía una carga base normal; durante el experimento se inyectaron múltiples procesos de laboratorio incrementando el conteo activo; y después de aplicar el comando de finalización, el sistema regresó a su estado inicial de forma limpia.

## ⚠️ Seguridad

¿Qué riesgos existen al administrar procesos?
El mayor riesgo es terminar por error un proceso crítico del sistema operativo o de servicios esenciales, lo que puede provocar inestabilidad en el equipo o pérdida de datos.

## 💭 Reflexión final

¿Qué aprendí sobre los procesos y la multiprogramación?
Comprendí que la administración de sistemas operativos no se trata meramente de memorizar comandos de terminal, sino de entender el ciclo de vida, la jerarquía y el consumo de recursos de cada tarea para intervenir de manera segura.
