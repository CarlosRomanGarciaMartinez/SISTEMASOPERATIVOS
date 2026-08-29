# 3. Identificar el sistema
*   **Distribución:** `cat /etc/os-release` (Ubuntu LTS).
*   **Arquitectura:** `uname -m` (x86_64).
*   **Usuario:** `whoami`.
Estos comandos permiten al administrador confirmar el entorno de trabajo y los privilegios actuales antes de operar el almacenamiento.

---

# 4. Observar los dispositivos
Al ejecutar `lsblk`, se detecta el dispositivo físico principal y sus ramificaciones lógicas. Se observa la relación jerárquica donde las particiones de menor tamaño dependen directamente del disco base.

---

# 6. Sistemas de archivos (`lsblk -f`)
*   **NAME:** Identificador del bloque.
*   **FSTYPE:** Formato (ext4, ntfs, vfat).
*   **LABEL:** Etiqueta humana.
*   **UUID:** Identificador persistente crucial para montajes automáticos.
*   **MOUNTPOINT:** Ruta de acceso en el árbol de directorios.

---

# 8. Investigar con `fdisk`
El comando `sudo fdisk -l` expone la tabla de particiones (GPT o MBR), el tamaño exacto en bytes/sectores, y los límites cilíndricos, permitiendo auditar la alineación y el esquema a bajo nivel.

---

# 9. Identificar las particiones del sistema
*   **🥾 Partición EFI:** Identificada por `vfat` y su bajo tamaño (~512MB).
*   **🪟 Partición de Windows:** Identificada por `ntfs`.
*   **🐧 Partición Linux:** Identificada por `ext4` y su punto de montaje en la raíz `/`.
