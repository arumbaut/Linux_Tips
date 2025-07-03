Vamos a cambiar de lugar el home de un usuario solicitado.

Primero nos conectamos a la pc con nuestro script 
```
conect IP
```

Hacemos un  para saber la info del usuario donde se hubica su home

```
cat /etc/pwd | grep username
```

Para saber la info del usuario donde se hubica su home

## Archivos relacionados (también existen en Red Hat)

|Archivo|Para qué sirve|
|---|---|
|`/etc/passwd`|Información de cuentas de usuario (no contraseñas).|
|`/etc/shadow`|Contraseñas encriptadas de los usuarios.|
|`/etc/group`|Información sobre los grupos del sistema.|
|`/etc/gshadow`|Contraseñas y permisos para grupos.|
Ejecutamos el comando para crear el nuevo directorio home y mover el contenido de el home antiguo

```
sudo usermod -d /nuevo/home/usuario -m usuario
```

