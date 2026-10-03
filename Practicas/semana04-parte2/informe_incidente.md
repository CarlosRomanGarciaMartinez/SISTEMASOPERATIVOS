# 🕵️‍♂️️ EL INFORME DEL INCIDENTE

## 1️⃣ ¿El servidor estaba expuesto?
Sí, el servidor presentaba una exposición masiva en la red de laboratorio. Durante el reconocimiento con `nmap` se encontraron más de 20 puertos abiertos, destacando servicios críticos como 21 (FTP), 22 (SSH), 23 (Telnet), 80 (HTTP), 3306 (MySQL), 5432 (PostgreSQL) y 5900 (VNC).

## 2️⃣ ¿Existía una cuenta conocida?
Sí, se detectó una cuenta con credenciales predeterminadas. Se realizó una prueba de acceso autorizada mediante SSH al puerto 22 utilizando las credenciales `msfadmin` como usuario y `msfadmin` como contraseña.

## 3️⃣ ¿Qué usuario obtuviste?
Se obtuvo acceso con el usuario `msfadmin`.
* Resultado de `whoami`: `msfadmin`
* Resultado de `id`: `uid=1000(msfadmin) gid=1000(msfadmin) groups=4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),107(fuse),111(lpadmin),112(admin),119(sambashare),1000(msfadmin)`
* Resultado de `groups`: `msfadmin adm dialout cdrom floppy audio dip video plugdev fuse lpadmin admin sambashare`

## 4️⃣ ¿Qué grupos existen?
Dentro del sistema existen múltiples grupos. Tres de los más destacados identificados fueron: `msfadmin` (grupo principal del usuario), `admin` (grupo administrativo que permite realizar tareas elevadas) y `adm` (otorga acceso a registros y logs del sistema).

## 5️⃣ ¿Qué permisos encontraste?
Al utilizar comandos como `ls -l` se identificaron diversas configuraciones. Se observaron permisos como `-rw-r--r--` para archivos de lectura general, directorios con `drwxr-xr-x` y, durante el experimento de seguridad, se aplicaron configuraciones restrictivas de `-rw-------` (`chmod 600`) para simular la protección de información confidencial.

## 6️⃣ ¿Qué archivos son especialmente sensibles?
Archivos como `/etc/passwd` y `/etc/shadow`. 
* El `/etc/passwd` contiene la estructura de usuarios y suele tener permisos de lectura general para que el sistema asocie los UIDs con nombres. 
* El `/etc/shadow` es extremadamente sensible porque almacena los hashes de las contraseñas. Tiene permisos mucho más restrictivos (`-rw-r-----`) para que nadie, excepto `root` o el grupo `shadow`, pueda consultarlo o modificarlo, previniendo ataques de fuerza bruta offline.

## 7️⃣ ¿Qué significa SUID?
El SUID (Set owner User ID) permite que un archivo ejecutable corra con los privilegios de su propietario (frecuentemente `root`) en lugar de los permisos de quien invoca el comando. Debe auditarse porque una vulnerabilidad en un archivo con SUID activado permite a un atacante estándar elevar sus privilegios y tomar control del sistema.

## 8️⃣ ¿Qué configuración de permisos consideras peligrosa?
Asignar permisos globales como `chmod 777` es sumamente peligroso. Esto otorga permisos absolutos de lectura (`r`), escritura (`w`) y ejecución (`x`) al Propietario, al Grupo y a "Otros". Significa que cualquier intruso en el sistema podría alterar archivos críticos, borrar datos o inyectar código malicioso sin restricciones.

## 9️⃣ ¿Cómo reducirías el riesgo?
1. Deshabilitar servicios innecesarios o en texto plano como Telnet, FTP y servicios VNC.
2. Cambiar inmediatamente las credenciales por defecto (`msfadmin`).
3. Eliminar usuarios de grupos como `admin` si no requieren privilegios elevados (principio de mínimo privilegio).
4. Configurar un firewall estricto para evitar la exposición de bases de datos como MySQL y PostgreSQL a la red completa.
5. Evitar usar `chmod 777` y auditar los archivos con SUID activo.

---

# 🛡️ PROPUESTA DE ENDURECIMIENTO

| Hallazgo | Riesgo | Acción recomendada | Prioridad |
|---|---|---|---|
| Múltiples puertos abiertos (VNC, FTP, DBs) | Crítico | Implementar firewall para cerrar todos los puertos excepto los necesarios de producción. | Alta |
| Puerto 22 (SSH) abierto con contraseña débil | Crítico | Cambiar contraseña por defecto e implementar autenticación por llaves SSH. | Alta |
| Puerto 23 (Telnet) abierto | Alto | Deshabilitar el servicio Telnet e instruir a los usuarios a usar SSH. | Alta |
| Archivos SUID que no deberían existir | Alto | Auditar binarios usando `find / -perm -4000`, retirando SUID con `chmod -s` si no se justifica. | Alta |
| Pertenencia a grupos administrativos (`admin`) | Medio | Revisar membresías de `msfadmin` y aplicar principio de mínimo privilegio. | Media |

---

# 🧠 PREGUNTAS DE INVESTIGACIÓN

### 1. ¿Qué diferencia existe entre autenticación y autorización?
La autenticación es el proceso de verificar la identidad de un usuario (ej. comprobar la contraseña de `msfadmin`). La autorización es validar qué recursos o acciones tiene permitidas ese usuario una vez que ya está dentro (ej. validar si sus permisos le dejan leer `/etc/shadow`).

### 2. ¿Qué diferencia existe entre usuario y grupo?
Un usuario representa una cuenta individual identificada por un UID. Un grupo (identificado por un GID) es un conjunto que agrupa a varios usuarios, lo que facilita a los administradores asignar permisos compartidos para directorios o archivos sin configurar usuario por usuario.

### 3. ¿Qué significa UID?
User Identifier. Es el número interno que utiliza el kernel de Linux para representar y gestionar a un usuario (los nombres son solo para lectura humana).

### 4. ¿Qué significa GID?
Group Identifier. Es el número interno que Linux utiliza para reconocer y gestionar grupos en el sistema.

### 5. ¿Qué usuario tiene UID 0?
El usuario `root`. Es un UID especial al que el kernel le otorga privilegios administrativos absolutos, ignorando las restricciones de permisos convencionales.

### 6. ¿Qué significa `rwx`?
Representa los permisos fundamentales del sistema de archivos en Linux: `r` (read/lectura), `w` (write/escritura) y `x` (execute/ejecución).

### 7. ¿Qué diferencia existe entre `chmod`, `chown`, `chgrp`?
`chmod` cambia los permisos de acceso (`rwx`) de un archivo. `chown` cambia al usuario propietario de ese archivo. `chgrp` cambia el grupo propietario del archivo.

### 8. ¿Qué es SUID?
Es un bit de permiso especial que hace que, al ejecutar un programa, el proceso adopte temporalmente los privilegios del propietario del archivo (generalmente `root`) en vez de los del usuario que ejecutó el comando.

### 9. ¿Por qué `/etc/shadow` debe estar protegido?
Porque contiene las contraseñas cifradas (hashes) de los usuarios. Si cualquier persona pudiera leerlo, podría copiar los hashes a su propia máquina y utilizar herramientas de fuerza bruta o diccionarios para descifrar las contraseñas.

### 10. ¿Por qué `chmod 777` puede ser peligroso?
Porque asigna control total (lectura, escritura y ejecución) a todos los usuarios del sistema (propietario, grupo y "otros"), eliminando cualquier tipo de barrera de seguridad o privacidad sobre el archivo o directorio.

### 11. ¿Por qué Telnet representa un riesgo frente a SSH?
Porque Telnet transmite la información, incluyendo los nombres de usuario y las contraseñas, en texto plano por la red. SSH soluciona este riesgo al encapsular y cifrar toda la comunicación mediante algoritmos criptográficos.

### 12. ¿Qué significa "principio de mínimo privilegio"?
Es una norma de seguridad que establece que a cada usuario, programa o proceso se le deben otorgar única y exclusivamente los permisos mínimos necesarios para cumplir su función, reduciendo la superficie de ataque en caso de una brecha.

---

# 🧠 REFLEXIÓN FINAL

### 💭 Pregunta 1: Si un servidor tiene un servicio abierto, ¿significa automáticamente que está comprometido?
No, no significa que esté hackeado. Significa que está "expuesto" escuchando peticiones en la red. Solo se compromete si el servicio abierto presenta una vulnerabilidad sin parchar o si utiliza credenciales débiles que un atacante logre adivinar.

### 💭 Pregunta 2: Si un usuario tiene una contraseña correcta, ¿significa automáticamente que puede hacer cualquier cosa?
No. La contraseña otorga el acceso inicial, pero sus capacidades están delimitadas por la autorización. Lo que el usuario pueda hacer dependerá de su UID, de los grupos a los que pertenece y de los permisos (`rwx`) asignados a los archivos.

### 💭 Pregunta 3: ¿Por qué los permisos son una segunda barrera después de la autenticación?
Porque asumen que una cuenta puede ser vulnerada. Si un atacante logra robar una contraseña y pasar la autenticación, los permisos internos impiden que modifique archivos de configuración críticos o acceda a la información de otros usuarios.

### 💭 Pregunta 4: ¿Qué podría ocurrir si todos los usuarios tuvieran privilegios administrativos?
El sistema perdería su integridad por completo. Cualquier usuario, de forma maliciosa o por accidente, podría modificar el `/etc/shadow`, borrar directorios raíz, detener servicios críticos o instalar malware con acceso total al kernel.

### 💭 Pregunta 5: ¿Cuál fue el hallazgo más importante de tu investigación?
Descubrir que el acceso inicial mediante puertos vulnerables o contraseñas débiles es solo la primera fase. Una vez dentro del sistema, el verdadero riesgo de que un atacante tome control total radica en malas configuraciones internas, como archivos con SUID activo, pertenencia injustificada a grupos administrativos y el abuso de permisos globales (777).
