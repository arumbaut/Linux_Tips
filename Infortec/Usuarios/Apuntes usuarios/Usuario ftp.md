1- Entramos en AOTLXPRFTP00001/00002 (sftpdsi.orange.es 83.231.36.99)

2- Creamos el grupo en ambas maquinas con el mismo GID, podemos buscar el grupo viendo usuarios con el home similar, si ya existe no lo creamos:

[root@aotlxprftp00001~] # groupadd -g XXX grupo

3- Creamos el usuario en ambas maquinas con el mismo UID

[root@aotlxprftp00001~] # useradd -d /sftp/$NOMBRE -s /sbin/nologin -c "ES/C/FTE/FTE/Usuario SFTP XXXXXXXXXX. Responsable, Nombre. WO000000000XXXXX" usuario

7- Creamos contraseña y le ponemos que no caduque nunca:

[root@aotlxprftp00001~] # passwd usuario

[root@aotlxprftp00001~] # chage -M -1 usuario

[root@aotlxprftp00001~] # chage -l usuario

8- Debemos hacer nosotros la primera conexion (dara error):

[root@aotlxprftp00001~] # sftp usuario@aotlxprftp0000X

9- Para solucionar este problema hacemos lo siguiente:

[root@aotlxprftp00001 ] # chmod 644 /sftp/$DIRECTORIO/etc/*

10- Pasamos la contraseña por correo y enviarle la peticion a RTV-TECSE RED DATOS