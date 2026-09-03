# Tecnológico Nacional de México (TecNM) Campus Rioverde
**Ingeniería en Sistemas Computacionales**
**Materia:** Sistemas Operativos
**Alumno:** Carlos Román García Martínez
**Práctica 02:** Procesos y multiprogramación en Ubuntu

---

## 🧠 Reto: "Soy el administrador de procesos"

**Reporte de Diagnóstico del Sistema**

**Contexto:** El usuario reporta que su computadora está lenta y solicita saber qué está pasando.

### 1. Observación Inicial
Inicié el diagnóstico utilizando la herramienta interactiva `top` para evaluar de manera global el comportamiento del sistema y detectar anomalías en la carga de la CPU y la memoria RAM.

### 2. Identificación del Problema (Proceso Investigado)
Mediante un ordenamiento por consumo con `ps aux --sort=-%cpu`, localicé el proceso que saturaba el sistema:
*   **PID:** `5210`
*   **Usuario propietario:** `carlos`
*   **Comando/Proceso:** `stress --cpu 4`
*   **Estado (STAT):** `R` (Running)
*   **Consumo de CPU:** `98.5%`
*   **Consumo de Memoria:** `1.2%`
*   **Proceso Padre (PPID):** `3142`
*   **Prioridad (NI):** `0`

### 3. Herramientas Utilizadas y Comprobación
Utilicé `ps aux` para auditar el origen del comando y consulté el directorio virtual `/proc/5210/status` para confirmar que se trataba de una prueba de estrés artificial en la ejecución de hilos de procesamiento.

### 4. Propuesta de Acción Administrativa
*   **Acción Apropiada:** Finalizar el proceso de manera controlada utilizando `kill 5210` tras verificar que no afectaba servicios esenciales del sistema operativo.
*   **Acción Peligrosa a Evitar:** Ejecutar un cierre forzoso indiscriminado (`kill -9`) sobre procesos del núcleo o utilizar un PID erróneo que pudiera comprometer la estabilidad del sistema.
