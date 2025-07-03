## GECOS DE USUARIO

El GECOS de los usuarios tiene el siguiente formato

```
ES/C/FTE/FTE/Adriano Hernández, César - WO0000001077056
```

Campos:

1- País puede ser ES (España) o PT (Portugal)

2 - Tipo de usuario

F: Funcional Es el usuario, en mayúsculas. Es un usuario usado por un grupo. La contraseña se da de alta en Cyberark. (Ej: root, oracle, weblogic)

 S: Subsistema / Servicios Son usuarios que usa el sistema para ejecutar aplicaciones. Estos usuarios no tienen login (Ej: Apache, sshd)

 C: Customer. Cliente  Son usuarios de las empresas clientes. La creación de estos nos llega a través de peticiones del ITSM. (Ej: FranceTelecom, Orange, Oesia)

 External  Usuarios de empresas que colaboran con kyndryl (Ej: Infortec)

 K: Kyndril Usuarios de la empresa kyndryl

3 - La empresa a la que pertenece (Ej: FTE, Orange…)

4 - Nombre del usuario seguido del número de WO en la que se pide (Puedes añadir el nombre de su responsable/persona que abre el ticket)

## USUARIOS QUE NO CADUCAN

Si en la petición indica que la contraseña del usuario no debe caducar, este debe indicar el motivo y pasamos la petición a seguridad (SOP_CIBER_IAM) para que ellos lo validen.

Para quitar la caducidad del usuario ejectuamos:

```
chage -M -1 usuario
```

Para comprobar que se ha desabilitado la caducidad del usuario ejecutamos:

```
chage -l  usuario
```

Nos conectamos a la maquina de destino y pasamos al modo su
```
ADM00001# sudo su -
```


Generamos un uid de usuario que no este en ninguna maquina asi como el grupo si en la peticion nos envian el grupo pues ya lo tenemos

Para generar los uid utilizamos el script 
```
usuid --user 
```

Esto nos da los UID disponibles para este usuario Tener en cuenta los que dicen KYNDRYL son para los usuarios de Sistemas o Admins hay que verificar

```
[root@AOTLXTEADM00001 ~]# usuid  
Ingrese el nombre de usuario: Usuario01  
No se encontraron coincidencias para el usuario Usuario01 en el directorio /evidencias/EVI_USU/.  
UID generado para el nuevo usuario CLIENTE Usuario01 (1000-6000): 1187 1193 1195 1197 1198 1199 1212 1214 1220 1224  
UID generado para el nuevo usuario KYNDRYL Usuario01 (6000-9999): 6008 6015 6016 6021 6028 6032 6033 6034 6035 6036  
GID generado para el nuevo usuario Usuario01 (1000-9999): 1040 1042 1043 1107 1111 1112 1114 1122 1123 1124

```

Si nos dan el Grupo pues verificamos el grupo con 
```
usuid --group
```

Para verificar el grupo id

Si todo va bien pues nos creamos el usuario 

```
useradd -m -u 1771 -g app -c "ES/C/FTE/FTE/{Grupoe de la peticion} {work order}" -s /bin/bash -d /homeuser2/usaocws usaocws

useradd -m -u 1771 -g app -c "ES/C/FTE/FTE/PROYECTOS_FT_OPE (WO0000001164017)" -s /bin/bash -d /homeuser2/usaocws usaocws
```

Este comando está creando un nuevo usuario llamado `usaocws` en un sistema Linux, con varias opciones específicas.

---

## 🧱 Explicación de cada parte

|Parte|Significado|
|---|---|
|`useradd`|Comando para crear un nuevo usuario en el sistema.|
|`-m`|Crea el **directorio home** del usuario si no existe.|
|`-u 1771`|Asigna el **UID (User ID)** 1771 al nuevo usuario.|
|`-g app`|Asigna al usuario al grupo primario llamado **`app`**. Este grupo debe existir previamente.|
|`-c "ES/C/FTE/FTE/..."`|Comentario descriptivo (usualmente para el campo "Full Name" o para identificar el propósito del usuario).|
|`-s /bin/bash`|Define la **shell por defecto** del usuario (`/bin/bash`).|
|`-d /homeuser2/usaocws`|Define una **ruta personalizada** para el directorio home. No es la habitual `/home/usaocws`, sino `/homeuser2/usaocws`.|
|`usaocws`|Nombre del nuevo usuario.|

Para la contraseña inicial usamos el script genpas en el nodo AOTLXTEADM00001
```
[root@AOTLXTEADM00001 ~]# genpass  
j2f8n95A10%REQY  
kPhiuRJ0EWSd7!4  
uD8wQjiloX5#f4K  
%975WNqH3il0681
```

### COMPROBACIÓN Y CREACIÓN DEL FS /homeuser2

NOTA: Como nuevo procediemiento asignamos espacio de cortesía del vg root al home de los usuarios

Como situación transitoria se asigna espacio del root_vg al home de los usuarios. El FS asignado se montará sobre la carpeta /homeuser2

Se comprueba si el FS /homeuser2 existe y si no lo creamos

```
[root@AOTLXPRWEB00028 ~]# df -h /homeuser2

```
Comrpobamos si hay espacio disponible en root_vg

```
[root@AOTLXPRWEB00028 ~]# vgs  VG       #PV #LV #SN Attr   VSize  VFree   
  datos_vg   1  26   0 wz--n- 32.00g   5.02g  
  ister_vg   1   1   0 wz--n-  1.94g 960.00m  
  root_vg    1  11   0 wz--n- 29.84g   9.09g  
  swap_vg    1   1   0 wz--n- 15.00g      0
```


Se crea el FS /homeuser2 con un espacio de cortesia de 64m para todos los home de usuario que creemos

Creacion del LV
```
lvcreate -n lv_homeuser2 64m root_vg  
```

Se crea `/dev/root_vg/lv_homeuser2` (o también accesible como `/dev/mapper/root_vg-lv_homeuser2`).

Formateamos el Volumen con formato ext4
```
mkfs.ext4 /dev/mapper/root_vg-lv_homeuser2 -m0  
mkdir -pv /homeuser2
```

Añadimos la entrada correspondiente a fstab y montamos el FS

```
WO000000XXXXXXX#/dev/mapper/datos_vg-lv_homeuser2  /homeuser2  ext4    defaults        0 2

```

Lo montamos 
```
mount /homeuser2
```

### CREACIÓN DEL GRUPO Y DEL USUARIO

Creamos el grupo y el usuario con los uid y gid anteriormente asignaos

```
groupadd -g 1113 Usuario01 

useradd  -m u 1043 -g 1113 -c "ES/C/FTE/FTE/Usuario para Aplicacion01 - WO000000XXXXXXX" -s /bin/bash -d /homeuser2/Usuario01 Usuario01

```

En caso de que sea para un usuario ftp la creacon del usuario varia nos fijamos en el  /bin/bash que es diferente.

```
useradd -m -u 1185 -g 1169 -c "ES/C/FTE/FTE/NEG_GGCC - WO0000001174469" -s /sbin/nologin -d /homeuser/user_ken user_ken
```

Asignamos una de las claves asignadas por el script genpass

```
passwd Usuario01
```

La contraseña se debe enviar al correo del peticionario o al que indique la petición. Usamos la plantilla “Credenciales en sistemas Linux (WO0000000XXXXXX)” del correo de grupo de atlas.linux@kyndryl.com

Si nos piden que no caduque la contraseña al terminar pasamos el siguiente comando.
```
chage -M -1 jenkins
chage -l jenkins
```
## Evidencias

Una vez realizada la petición añadimos en esta las evidencias de la creación del usaurio.

Añadiremos estas evidencias en la resolución de la petición

```
tail -1 /etc/passwdgroups Usuario01  
ls -ld /homeuser2/Usuario01ram  
chage -l Usuario01
```

Es importante verificar lo de donde se va a crear el home porque es probable que se deba crear el LVM donde se guardara este home de usario . Pendiente a preguntar ❓