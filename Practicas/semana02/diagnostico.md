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
*[Explica cómo iniciaste el diagnóstico. Por ejemplo: "Al recibir el reporte, decidí utilizar la herramienta `top` para tener una vista en tiempo real del consumo de recursos del sistema..."]*

### 2. Identificación del Problema (Proceso Investigado)
*[Detalla el proceso que localizaste. Ejemplo: "Identifiqué que un proceso estaba consumiendo el 90% de la CPU..."]*
*   **PID:** *[Número]*
*   **Usuario propietario:** *[Nombre]*
*   **Comando/Proceso:** *[Nombre del comando]*
*   **Estado (STAT):** *[Estado]*
*   **Consumo de CPU:** *[Porcentaje]*
*   **Consumo de Memoria:** *[Porcentaje]*
*   **Proceso Padre (PPID):** *[Número de PPID, investigado con `ps -p [PID] -o ppid` o `pstree`]*
*   **Prioridad (NI):** *[Valor de nice]*

### 3. Herramientas Utilizadas y Comprobación
*[Explica las herramientas exactas. Ejemplo: "Utilicé `ps aux --sort=-%cpu` para confirmar que efectivamente era el proceso con mayor carga. Luego utilicé `cat /proc/[PID]/status` para..."]*

### 4. Propuesta de Acción Administrativa
*   **Acción Apropiada:** *[¿Qué recomendarías hacer? ¿Cambiarle la prioridad con `renice`? ¿Contactar al usuario? ¿Dejar que termine?]*
*   **Acción Peligrosa a Evitar:** *[Por ejemplo, "Sería peligroso ejecutar un `kill -9` de inmediato sin saber si el proceso pertenece a un servicio crítico del sistema, ya que podría corromper datos o detener el entorno gráfico."]*
