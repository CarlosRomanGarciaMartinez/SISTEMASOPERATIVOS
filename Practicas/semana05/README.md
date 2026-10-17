# 🟣⚙️ Semana 05 — Procesos en Ubuntu 🐧

## 👤 Datos del alumno
- 🧑‍💻 **Nombre:** Carlos Román García Martínez
- 📅 **Fecha:** 17 de octubre de 2026

## 💻 Entorno de trabajo
- 🐧 **Distribución y versión de Ubuntu:** Ubuntu Linux (Dual-Boot)
- 🖥️ **¿Equipo físico o máquina virtual?:** Equipo físico
- ⌨️ **Terminal utilizada:** GNOME Terminal (`/dev/pts/0` - `/bin/bash`)

## 🎯 Objetivo
Observar y administrar procesos de prueba en Ubuntu desde la terminal, identificando sus identificadores (`PID` y `PPID`), estados de ejecución (`STAT`), uso de recursos y prioridad (`NI`); experimentando con el control de trabajos en primer y segundo plano (`jobs`, `fg`, `bg`, `Ctrl+C`, `Ctrl+Z`) y aplicando señales de terminación segura (`SIGTERM`).

## 🛠️ Comandos principales
| ⌨️ Comando | 📖 ¿Qué hace? | 🔍 Resultado observado |
|---|---|---|
| `ps` | 📋 Lista los procesos asociados únicamente a la terminal actual. | Mostró el proceso `bash` y el propio comando `ps` con sus `PID`, `TTY`, `TIME` y `CMD`. |
| `ps aux` | 🌐 Muestra una instantánea de todos los procesos del sistema de todos los usuarios. | Desplegó la tabla completa con columnas `USER`, `PID`, `%CPU`, `%MEM`, `VSZ`, `RSS`, `STAT`, `START`, `TIME` y `COMMAND`. |
| `pgrep` | 🔎 Busca procesos activos filtrando por patrón de nombre (con `-a` muestra el comando completo). | Con `pgrep -a bash` y `pgrep -a -x sleep` devolvió los `PID` y líneas de comando exactas. |
| `top` | 📊 Muestra en tiempo real los procesos activos y el resumen de uso de CPU y memoria. | Permitió observar dinámicamente qué procesos cambiaban su consumo de `%CPU` y `%MEM` cada segundo. |
| `jobs -l` | 🗂️ Lista los trabajos iniciados y administrados por la sesión actual del shell, incluyendo su `PID`. | Mostró los procesos `sleep` en ejecución (`Running`) o suspendidos (`Stopped`) junto con su número de trabajo `[1]`, `[2]` y `PID`. |
| `nice` | 🎚️ Ejecuta un programa asignándole un valor de prioridad de planificación inicial (`NI`). | Con `nice -n 10 sleep 300 &` inició un proceso de prueba con valor `NI` igual a `10` (menor prioridad relativa). |
| `kill -TERM` | 🛑 Envía la señal `SIGTERM` (15) a un `PID` específico para solicitar su terminación ordenada. | Finalizó limpiamente el proceso `sleep` de prueba sin afectar otros procesos del sistema. |

## 🔬 Hallazgos principales
- 🆔 **PID del proceso de prueba:** `[ej. 24680]`
- 🌳 **PPID (Proceso Padre):** `[ej. 21450]` (correspondiente al intérprete `/bin/bash` de la terminal activa)
- 💤 **Estado observado (`STAT`):** `S` (En espera interrumpible / *Interruptible sleep*)
- ⚖️ **Valor `NI` (Prioridad):** `0` en el proceso normal y `10` en el proceso lanzado con `nice -n 10`
- ✅ **¿Cómo comprobé su terminación?:** Ejecutando `ps -p PID -o pid,stat,cmd` (el cual dejó de listar el proceso en la tabla de procesos) y verificando el aviso `Terminated` mediante `jobs -l`.

## 📸 Evidencias fotográficas

### 🖼️ 1. Reconocimiento de sesión y tiempo activo
> 📝 **Descripción:** Muestra la identificación del usuario efectivo (`whoami`), la terminal conectada (`tty`), el shell activo (`echo "$SHELL"`) y el tiempo de actividad junto con los promedios de carga del sistema (`uptime`).

![Sesión y uptime](evidencias/01_sesion_uptime.png)

### 🖼️ 2. Consulta general y filtrada de procesos
> 📝 **Descripción:** Muestra la tabla global de procesos del sistema mediante `ps aux` y la localización específica del proceso `bash` con `pgrep -a bash`.

![Consulta ps aux y pgrep](evidencias/02_ps_aux.png)

### 🖼️ 3. Monitoreo en tiempo real con `top`
> 📝 **Descripción:** Muestra la vista dinámica en tiempo real del uso de CPU, memoria RAM/Swap y la tabla interactiva de procesos activos en `top`.

![Monitor top en tiempo real](evidencias/03_top.png)

### 🖼️ 4. Identificación de PID, PPID y estado de un proceso de prueba
> 📝 **Descripción:** Muestra la creación del proceso controlado `sleep 300 &`, su registro en `jobs -l` y la consulta de su `PID`, `PPID`, `STAT` y `NI` mediante `ps`.

![Proceso de prueba PID y PPID](evidencias/04_proceso_pid_ppid.png)

### 🖼️ 5. Control de trabajos en primer y segundo plano
> 📝 **Descripción:** Muestra la suspensión de un trabajo en primer plano con `Ctrl+Z` (estado `Stopped`) y su reanudación en segundo plano con `bg` verificada mediante `jobs -l`.

![Control de trabajos con jobs](evidencias/05_jobs.png)

### 🖼️ 6. Modificación de prioridad con `nice`
> 📝 **Descripción:** Muestra el proceso lanzado con `nice -n 10 sleep 300 &` y la verificación de la columna `NI` establecida en `10` usando `ps`.

![Prioridad nice del proceso](evidencias/06_prioridad_nice.png)

### 🖼️ 7. Terminación controlada con `SIGTERM`
> 📝 **Descripción:** Muestra la verificación previa del `PID` del proceso `sleep`, el envío de la señal con `kill -TERM PID` y la comprobación posterior de que el proceso finalizó.

![Terminación con SIGTERM](evidencias/07_terminacion_sigterm.png)

## 🏁 Conclusión
💡 A través de esta práctica comprobé que el sistema operativo administra múltiples procesos en segundo plano aunque no tengan una ventana abierta. Aprendí a diferenciar una consulta estática (`ps`) de una dinámica (`top`), a gestionar trabajos dentro de la sesión de Bash y a verificar siempre el `PID` y el comando exacto antes de enviar señales de terminación como `SIGTERM`, evitando así interrumpir procesos críticos del sistema o de otros usuarios.
