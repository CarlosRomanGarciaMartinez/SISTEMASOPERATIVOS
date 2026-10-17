# 🟢🧠 Semana 06 — Memoria y recursos en Ubuntu 🐧

## 👤 Datos del alumno
- 🧑‍💻 **Nombre:** Carlos Román García Martínez
- 📅 **Fecha:** 17 de octubre de 2026

## 💻 Entorno de trabajo
- 🐧 **Distribución y versión:** Ubuntu Linux (Dual-Boot)
- 🖥️ **Equipo físico o máquina virtual:** Equipo físico
- ⚙️ **CPU lógicas (`nproc`):** `[ej. 8 o 12]`

## 🎯 Objetivo
Analizar desde la terminal de Ubuntu el uso de memoria RAM, memoria virtual, espacio swap, carga promedio del sistema y consumo de recursos por proceso, comparando mediciones antes, durante y después de una tarea controlada e inspeccionando el sistema de archivos virtual `/proc`.

## 📊 Medición inicial
| 📌 Indicador | 🔢 Resultado | 📖 Interpretación |
|---|---|---|
| 🧮 **RAM total** | `[ej. 15Gi]` | Memoria física total reconocida por el kernel. |
| 🟢 **RAM disponible (`available`)** | `[ej. 11Gi]` | Memoria estimada disponible para iniciar nuevas aplicaciones sin recurrir a swap (incluye caché recuperable). |
| 💾 **Swap total** | `[ej. 2.0Gi]` | Espacio en disco configurado como respaldo de páginas de memoria. |
| 🔄 **Swap usada** | `[ej. 0B]` | Cantidad de swap utilizada actualmente; al haber suficiente RAM disponible, su uso es nulo o mínimo. |
| 📈 **Carga promedio** | `[ej. 0.45, 0.38, 0.30]` | Promedio de tareas ejecutables o en espera a 1, 5 y 15 minutos; muy por debajo del número de CPU lógicas. |

## 🔎 Procesos observados
- 🔥 **Mayor uso de CPU observado:** `[ej. gnome-shell / navegador]` (`[ej. 4.5%]`)
- 🐘 **Mayor uso de memoria observado:** `[ej. firefox / code]` (`[ej. 8.2%]`)
- 🆔 **PID analizado:** `[ej. 21450]` (`bash`)
- 📂 **¿Qué información consulté en `/proc`?:** Consulté `/proc/cpuinfo` para ver el modelo del procesador, `/proc/meminfo` para contrastar las métricas de RAM y Swap en kB, `/proc/uptime` para el tiempo activo en segundos, y `/proc/PID/status` para verificar `Name`, `State`, `Pid`, `PPid`, `VmRSS`, `VmSize` y `Threads`.

## 🧪 Experimento antes / durante / después
⏱️ Durante la ejecución controlada de `sleep 120`, los valores de memoria disponible (`available`), swap usada (`0B`) y tiempo de CPU inactiva (`id` en `vmstat`) permanecieron prácticamente estables. Esto demuestra técnicamente que un proceso en espera (`S`) no degrada los recursos del sistema y que tener procesos abiertos no implica automáticamente un alto consumo de RAM o CPU.

## 🩺 Diagnóstico
💡 Ante el caso de un usuario que afirma que su equipo está lento porque "la RAM está al 90%", se comprobó con `free -h` y `vmstat 1 5` que Linux aprovecha la memoria libre para `buff/cache` con el fin de acelerar el sistema, y que el indicador real a vigilar es `available` junto con la actividad de transferencia de swap (`si` y `so` en `vmstat`).

## 📸 Evidencias fotográficas

### 🖼️ 1. Medición inicial de RAM, tiempo activo y CPU lógicas
> 📝 **Descripción:** Muestra el estado base del sistema mediante los comandos `free -h`, `uptime` y `nproc`.

![Medición inicial free uptime nproc](evidencias/01_free_uptime_nproc.png)

### 🖼️ 2. Consulta directa a `/proc/meminfo`
> 📝 **Descripción:** Muestra las primeras 20 líneas de `/proc/meminfo` con el desglose detallado de memoria en `kB`.

![Detalle de /proc/meminfo](evidencias/02_meminfo.png)

### 🖼️ 3. Monitoreo por intervalos con `vmstat 1 5`
> 📝 **Descripción:** Muestra 5 muestras consecutivas de 1 segundo de procesos (`r`, `b`), memoria, paginación (`si`/`so`) y uso de CPU (`us`, `sy`, `id`, `wa`).

![Salida de vmstat 1 5](evidencias/03_vmstat.png)

### 🖼️ 4. Procesos ordenados por consumo de memoria RAM
> 📝 **Descripción:** Muestra los procesos con mayor porcentaje de uso de memoria (`%MEM`) usando `ps aux --sort=-%mem | head -n 10`.

![Top procesos por memoria](evidencias/04_procesos_memoria.png)

### 🖼️ 5. Procesos ordenados por consumo de CPU y detalle de un PID
> 📝 **Descripción:** Muestra los procesos con mayor porcentaje de uso de CPU (`%CPU`) y la consulta detallada de `RSS` y `VSZ` de un `PID` específico.

![Top procesos por CPU y detalle PID](evidencias/05_procesos_cpu.png)

### 🖼️ 6. Experimento controlado — Medición A (Antes)
> 📝 **Descripción:** Muestra la marca de tiempo (`date`), memoria (`free -h`) y comportamiento de CPU (`vmstat 1 3`) antes de iniciar el proceso de prueba.

![Comparación Antes](evidencias/06_comparacion_antes.png)

### 🖼️ 7. Experimento controlado — Medición B (Durante)
> 📝 **Descripción:** Muestra la memoria (`free -h`), el proceso `sleep 120` activo con `ps -C sleep`, `vmstat 1 3` y `uptime` durante la prueba.

![Comparación Durante](evidencias/07_comparacion_durante.png)

### 🖼️ 8. Experimento controlado — Medición C (Después)
> 📝 **Descripción:** Muestra los resultados de `free -h`, `uptime` y la verificación con `pgrep -a -x sleep` tras finalizar el proceso de prueba.

![Comparación Después](evidencias/08_comparacion_despues.png)

### 🖼️ 9. Exploración de `/proc/meminfo` y `/proc/PID/status`
> 📝 **Descripción:** Muestra el filtrado con expresiones regulares (`grep -E`) sobre `/proc/meminfo` y los atributos internos de un proceso en `/proc/PID/status`.

![Consulta en /proc/PID/status](evidencias/09_proc_pid_status.png)

## 🏁 Conclusión
✅ Aprendí a distinguir entre memoria `free` y memoria `available`, comprendiendo que Linux utiliza la RAM libre para caché y buffers sin que eso signifique falta de memoria. Asimismo, aprendí a relacionar la carga promedio (`load average`) con el número de CPU lógicas (`nproc`) y a respaldar cualquier diagnóstico de rendimiento con mediciones reales de `vmstat`, `ps` y `/proc`.
