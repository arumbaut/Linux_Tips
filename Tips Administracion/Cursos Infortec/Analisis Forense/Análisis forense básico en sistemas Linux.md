Los logs se encuentran principalmente

cd /var/log

servicios especificos pueden instalarse en la carpeta de usuario 

Archivos de registro comunes (pueden variar según la distro):
```
/var/log/message: registro de mensajes generales del sistema 
/var/log/auth.log: log de autenticación /var/log/kern.log: registro del kernel /var/log/cron.log: registro de crond /var/log/maillog: registro del servidor de mails /var/log/qmail/ : registro de Qmail /var/log/httpd/: registro de errores y accesos a Apache
/var/log/lighttpd: registro de errores y accesos a Lighttpd 
/var/log/boot.log : registro de inicio del sistema /var/log/mysqld.log: registro de la base de datos MySQL 
/var/log/secure: log de autenticación /var/log/utmp or /var/log/wtmp : registro de logins
```


Ver listado de pack instalados

```
dpkg-query -l
```

Si solo deseamos ver el listado de paquetes sin ninguna descripción ejecutaremos lo siguiente: 
```
dpkg-query -f '${binary:Package}\n' -W
```

También podemos recurrir a la siguiente línea: 
```
apt list –installed
```

Si por alguna razón deseamos que los resultados obtenidos sean redireccionados y almacenados a un archivo de texto para su posterior análisis, vamos a ejecutar lo siguiente: 
```
dpkg --get-selections | grep -v deinstall > ejemplo.txt>
```

También podemos usar la siguiente línea:
```
dpkg -l | grep ^ii | awk ‘{ print $2}’ > mylist.txt
```

Es normal que determinados paquetes ya no sean mas usados en Linux y para ello pueden ser eliminados, esto liberara espacio en el disco, para esta tarea vamos a ejecutar lo siguiente: 
```
sudo apt-get autoremove
```

Firmar los ficheros con algoritmo sha512 ejemplo el fichero sudoers para ver que no ha sufrido modificacniones.
```
sha512 evidencia_sudoers > firmas.txt
```

Comparacion de presencia de lineas
```
grep -Fxf archivo1.txt archivo2.txt
```

Buscar permisos 444
```
sudo find -type f -perm -444  > 

sudo find -type d -perm -111  <<sudo find -type f -perm -444
```

Esto se hace usuario por usuario parar pododer ver los logs tirados en el  sitema de l usuario sospechoso
```

cat /home/user/.bash_history
```

Net Tools permite manejo de red mediante paketes

```
netstat -an 
```

Muy importante tener una copia del fichero boot.log y tener encuenta que systemas levanta con el arranque.