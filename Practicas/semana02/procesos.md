# Tecnológico Nacional de México (TecNM) Campus Rioverde
**Ingeniería en Sistemas Computacionales**
**Materia:** Sistemas Operativos
**Práctica 02:** Procesos y multiprogramación en Ubuntu

---

## 🧪 Registro de Laboratorio y Experimentación

### 1. Entorno de Trabajo y Proceso Actual
*   **Comando utilizado para usuario:** `whoami` -> Resultado: *[Tu resultado]*
*   **Comando utilizado para PID de shell:** `echo $$` -> Resultado: *[Tu resultado]*
*   **Pregunta:** ¿Por qué una terminal puede considerarse relacionada con un proceso?
    *   *[Tu respuesta]*

![Entorno](./evidencias/01_entorno.png)

### 2. Observación de Procesos del Sistema (`ps` y `ps aux`)
*   **¿Cuántos procesos aparecen?** *[Respuesta]*
*   **¿Qué proceso consume más CPU y cuál es su PID?** *[Respuesta]*
*   **¿Qué proceso consume más memoria y cuál es su PID?** *[Respuesta]*
*   **Pregunta:** ¿El proceso que más CPU utiliza necesariamente es el proceso que más memoria utiliza?
    *   *[Justificación]*

![Comando ps](./evidencias/02_ps.png)
![Comando ps aux](./evidencias/03_ps_aux.png)

### 3. Relación Padre-Hijo (`pstree`)
*   **Proceso Padre Identificado:** *[Nombre y PID]*
*   **Proceso(s) Hijo(s) Identificado(s):** *[Nombre y PID]*
*   **Pregunta:** ¿Por qué es útil conocer quién creó a un proceso?
    *   *[Tu respuesta]*

**Representación gráfica del reto:**
    🟦 [Nombre Proceso Padre] (PID: XXXX)
          │
          ├── 🟩 [Nombre Proceso Hijo 1] (PID: YYYY)
          │
          └── 🟨 [Nombre Proceso Hijo 2] (PID: ZZZZ)

![Árbol de procesos](./evidencias/04_pstree.png)

### 4. Observación en Tiempo Real (`top`)
*   **Pregunta:** ¿Qué diferencia existe entre observar una fotografía de los procesos y observarlos en tiempo real?
    *   *[Tu respuesta]*

![Comando top](./evidencias/05_top.png)

### 5. Creación y Ciclo de Vida de un Proceso (`sleep 300`)
*   **Antes y Durante:**
    *   PID del proceso creado: *[PID]*
    *   PPID: *[PPID]*
    *   Estado: *[Estado]*
*   **Pregunta:** ¿Qué información permanece igual mientras el proceso continúa ejecutándose y qué información podría cambiar?
    *   *[Tu respuesta]*
*   **Finalización (`kill`):**
    *   Comando ejecutado: `kill [Tu_PID]`
*   **Pregunta:** ¿Por qué es peligroso ejecutar una orden de finalización sin comprobar primero el PID?
    *   *[Tu respuesta]*
*   **Pregunta:** Si ejecutamos tres veces el mismo comando, ¿tenemos un programa o tres procesos?
    *   *[Tu explicación]*

![Proceso de laboratorio](./evidencias/06_proceso_lab.png)

### 6. Administración de Trabajos (Background y Foreground)
**Secuencia de trabajo:**
1.  ▶️ Ejecutar proceso: `sleep 120 &`
2.  📋 Identificar trabajo: `jobs`
3.  ⬇️ Enviar a segundo plano: `bg` (si aplicó)
4.  ⬆️ Recuperarlo: `fg %1`
5.  🛑 Finalizarlo: `Ctrl + C` o `kill`

![Segundo plano](./evidencias/07_segundo_plano.png)

### 7. Experimento con `nice`
*   **Comando utilizado:** `nice -n [valor] sleep 300 &`
*   **PID:** *[PID]*
*   **Valor NI observado:** *[Valor]*
*   **Resultado observado:** *[Descripción]*

![Prioridad de procesos](./evidencias/08_prioridad.png)

### 8. Investigación mediante `/proc`
*   **Directorio consultado:** `/proc/[PID]/status`
*   **Información encontrada:** *[Describe qué viste, como nombre, estado, Uid, etc.]*

![Directorio proc](./evidencias/09_proc.png)

### 9. Multiprogramación (Antes, Durante y Después)
*   **Antes:** Cantidad aproximada de procesos *[Número]*
*   **Durante (ejecutando varios `sleep 300 &`):**
    *   Nuevos PIDs generados: *[Lista de PIDs]*
    *   Comprobación de coexistencia: *[Explicación]*
*   **Después:** Comprobación de que terminaron correctamente.
*   **Pregunta:** ¿Qué problema tendría una computadora si solamente pudiera mantener un proceso activo durante toda su operación?
    *   *[Tu respuesta]*

![Multiprogramación](./evidencias/10_multiprogramacion.png)

### 10. Tabla Comparativa de Herramientas

| Herramienta | ¿Qué observé? | ¿Qué información obtuve? | ¿Para qué me sirve? |
|---|---|---|---|
| `ps` | | | |
| `ps aux` | | | |
| `pstree` | | | |
| `top` | | | |
| `jobs` | | | |
| `nice` | | | |
| `/proc` | | | |
