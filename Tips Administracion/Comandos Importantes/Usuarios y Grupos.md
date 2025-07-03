Crear Usuario
```
sudo useradd -c "Descripcion" -m lolo
#-m para crear el home
#seguidamente le asignamos una password
sudo passwd lolo
```

Crear grupos
```
groupadd Linux1
```

Agregar un usuario a un grupo 
```
sudo usermod -aG Linux1 Linux2 rene
#-a grega a la lista de grupos del usuario si no lo ponemos sobreescribe y solo pone el grupo que indiquemos en el comando
#-G la lista suplementaria de grupos
#-g fuerza el grupo como grupo primario no es necesario poner -a solo modifica el primario
```

Para ver la informacion de los grupos de un usuario
```
id lolo
```

Que diferencia existe en un usuario pertnecer a un grupo primario y los secundarios

En los sistemas tipo Unix (como Linux), los usuarios pueden pertenecer a **grupos** para gestionar permisos de acceso a archivos y recursos. Existen dos tipos de pertenencia a grupos:

### 1\. Grupo primario

- Es el grupo principal al que pertenece un usuario.
- Cada usuario tiene **un solo grupo primario**.
- Cuando el usuario crea un archivo, ese archivo pertenece por defecto a su grupo primario (a menos que haya configuraciones especiales como `setgid` en directorios).
- Se define en el archivo `/etc/passwd`.

### 2\. Grupos secundarios

- Son grupos **adicionales** a los que puede pertenecer un usuario.
- Un usuario puede pertenecer a **varios grupos secundarios**.
- Sirven para conceder acceso adicional a recursos que pertenecen a esos grupos (por ejemplo, poder leer o ejecutar archivos de otro grupo).
- Se definen en el archivo `/etc/group`.

---

### Ejemplo:

Supongamos que el usuario `maria` tiene:

- Grupo primario: `maria`
- Grupos secundarios: `audio`, `video`, `docker`

Esto significa que:

- Cuando `maria` crea un archivo, será del grupo `maria`.
- Pero también podrá acceder a archivos o recursos que pertenecen a los grupos `audio`, `video` o `docker`, siempre que los permisos lo permitan.

---

### ¿Por qué es importante la diferencia?

- **Gestión de permisos**: El grupo primario influye en la propiedad por defecto de archivos creados.
- **Acceso compartido**: Los grupos secundarios permiten participar en distintos entornos colaborativos o acceder a funcionalidades específicas (como audio o virtualización).
- **Seguridad**: Permite controlar qué recursos puede usar cada usuario sin dar acceso completo al sistema.

Estás creando efectivamente el usuario **`pepe`** con su directorio home (`-m`) y un comentario (`-c`). Sin embargo, **no estás estableciendo una contraseña** para ese usuario, por lo que:

- El usuario existe.
- Pero **no puede iniciar sesión (ni con `su pepe` ni por otros medios)** hasta que se le asigne una contraseña.

---

### ¿Por qué su rene pide contraseña y no puedes entrar?

Porque `su rene` (o `su pepe`) **cambia de usuario**, y para eso te pide la **contraseña del usuario destino** (`rene`, en tu ejemplo). Si ese usuario no tiene contraseña asignada (o la desconoces), no puedes cambiarte a él con `su`.

---

### ✅ Solución: Asignar una contraseña al usuario

Para establecer una contraseña, usa:

Y luego te pedirá que escribas y confirmes la nueva contraseña.

---

### Extra: ¿Cómo crear el usuario y definir su contraseña desde el inicio?

Podrías hacerlo en dos pasos, como:

O, si prefieres una alternativa más interactiva que **sí pide la contraseña al crear el usuario**, puedes usar `adduser` (más amigable que `useradd`):

Este comando:

- Crea el usuario.
- Crea el home.
- Solicita contraseña.
- Configura datos adicionales.

---

¿Quieres que revisemos también el usuario `rene`, o necesitas eliminar y volver a crear algún usuario correctamente?

ver los grupos en los que esta un usuario y cal es el principal

Para ver **los grupos a los que pertenece un usuario** (incluyendo cuál es su **grupo primario**), puedes usar los siguientes comandos en Linux:

---

### ✅ 1. Ver todos los grupos de un usuario (incluyendo el actual si estás logueado):

**Ejemplo:**

Salida posible:

- El primer grupo (`pepe`) es el **grupo primario**.
- Los demás (`audio`, `video`, `docker`) son **grupos secundarios**.


A una carpeta se le puede dar permisos por grupos

¡Sí! En Linux puedes **dar permisos a una carpeta según el grupo** al que pertenezca. Esto es una de las funciones más comunes del sistema de permisos en Unix/Linux.

---

## 🔒 Permisos básicos de una carpeta

Cada archivo o carpeta en Linux tiene permisos para tres tipos de usuarios:

1. **Propietario (owner)**
2. **Grupo (group)**
3. **Otros (others)**

Puedes ver los permisos con:
```
ls -ld PruebaG/
drwxr-xr-x 2 adrian adrian 4096 Jun 17 16:37 PruebaG/
#Te dice el propietario y el grupo al que pertenece
```

Cambiar al grupo propietario a una carpeta
```
sudo chgrp Linux3 PruebaG/
#Parametros 1 Nombre del Grupo 2 Carpeta a la que se aplica el cambio
```

Cambiar los permisos
- `r`: leer                                                    4
- `w`: escribir (crear/modificar archivos)     2
- `x`: acceder (entrar en el directorio)        1

```
 chmod 750 PruebaG/
 # 7 permisos de propietario 4+2+1  (rwx)
 # 5 permisos del grupo propietario 4+1  (rx)
 # 0 permisos para otros 0  (ningun permiso)
```
### 3\. Hacer que los nuevos archivos hereden el grupo del directorio (opcional pero útil):

Esto aplica el **bit setgid**, que fuerza que los nuevos archivos/direcciones creados dentro de la carpeta pertenezcan al **grupo del directorio**.


### 🔹 ¿Qué significa esto?

Cada archivo o carpeta en Linux tiene:

- **Un propietario (usuario)**
- **Un grupo propietario (único)**


---

