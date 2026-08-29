# 1. Investigación previa

**¿Qué es un HDD?**
Un disco duro mecánico (Hard Disk Drive) almacena datos en platos magnéticos giratorios, dependiendo de partes móviles que limitan su velocidad de lectura y escritura.

**¿Qué es un SSD?**
Una unidad de estado sólido (Solid State Drive) utiliza memoria flash sin partes móviles, ofreciendo tiempos de acceso casi instantáneos y mayor resistencia.

**¿Qué es un NVMe?**
Un protocolo de comunicación que conecta el almacenamiento flash directamente a través del bus PCIe, maximizando el paralelismo y reduciendo la latencia.

**¿Qué diferencia existe entre un disco y una partición?**
El disco es el dispositivo físico completo. Una partición es una división lógica de ese disco, gestionada por el sistema operativo para segmentar el almacenamiento.

**¿Qué es un sistema de archivos?**
La estructura lógica que determina cómo se nombran, almacenan y organizan los datos dentro de una partición.

**¿Qué es NTFS?**
El sistema de archivos predeterminado de Windows, diseñado para manejar permisos avanzados, cifrado y archivos de gran tamaño.

**¿Qué es ext4?**
El sistema de archivos estándar en entornos Linux, optimizado para evitar la fragmentación y mantener la integridad mediante journaling.

**¿Qué es UEFI?**
La interfaz de firmware moderna que reemplaza al BIOS, acelerando el arranque y soportando particiones de gran tamaño.

**¿Qué es GPT?**
Una tabla de particiones moderna que utiliza identificadores únicos, soporta discos mayores a 2 TB y permite múltiples particiones primarias.

**¿Qué es MBR?**
El esquema de particionamiento heredado, limitado a 4 particiones primarias y 2 TB de capacidad.

---

# 2. Conocer el entorno

**¿Qué es una terminal?**
Una interfaz de línea de comandos para interactuar directamente con el sistema.

**¿Qué es Bash?**
El intérprete de comandos por defecto en la mayoría de distribuciones Linux.

**¿Qué significa ejecutar un comando?**
Enviar una instrucción de texto para que el sistema realice una tarea específica.

**¿Qué es la salida estándar?**
El lugar predeterminado donde un comando muestra su resultado, generalmente la pantalla.

**¿Qué significa utilizar `sudo`?**
Ejecutar una instrucción con privilegios temporales de superusuario.

---

# 5. Investigar `lsblk`
`lsblk` lista los dispositivos de bloques conectados (discos y particiones) mostrando su estructura en formato de árbol, tamaños y puntos de montaje[cite: 1]. Linux identifica los discos con nomenclaturas como `sda` o `nvme0n1`, y las particiones agregando números (ej. `sda1`).

---

# 7. Identificar sistemas de archivos
*   **ext4**: Nativo de Linux, usado para el sistema operativo.
*   **vfat (FAT32)**: Universal, indispensable para la partición EFI de arranque.
*   **ntfs**: Usado por Windows para sus datos y sistema.
*   *Pregunta de análisis:* Un mismo equipo utiliza varios sistemas porque la placa base requiere FAT32 para iniciar (EFI), mientras que cada sistema operativo (Linux/Windows) necesita su formato nativo para gestionar permisos correctamente.

---

# 10. Windows, NTFS y BitLocker
Windows utiliza NTFS por su seguridad mediante ACLs y prevención de corrupción de datos. Se identifica por la etiqueta NTFS o su tamaño asociado a C:. Modificarla desde Linux conlleva riesgo de corrupción. BitLocker cifra todo el volumen; si se altera la partición sin la clave, los datos se pierden permanentemente.
*   *Pregunta de análisis:* No se puede tratar como una partición normal. El cifrado oculta la estructura; cualquier redimensionamiento destruirá los metadatos y la hará irrecuperable.

---

# 17. Particionar no es formatear
**Particionar** es delimitar las fronteras físicas o lógicas en el disco. **Formatear** es inyectar un sistema de archivos dentro de esa frontera para poder guardar información. Sí puede existir una partición sin formato (RAW), pero el sistema operativo no podrá usarla hasta formatearla.
