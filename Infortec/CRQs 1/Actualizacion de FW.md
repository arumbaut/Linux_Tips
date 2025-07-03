Desactivar la monitorizacion sino lo esta ya:
https://es01pap00cs00xg:9444/Portal/facelets/home.xhtml

En la RSA nos guardamos las versiones del FW para compararlas despues del cambio
Antes de parar la maquina sacamos un crqcheck
Lanzamos el comando:
```
read -p "Usuario:" user_mod ; echo "usermod -p \"$(grep "^${user_mod}:" /etc/shadow | cut -d: -f2 | sed 's/\$/\\$/g')\" ${user_mod}"
```

Guardamos el resultado ya que sera la passwd de root que tendremos que poner posteriormente
Lanzamos el comando:
```
dmidecode | grep "Version"
```
Guardamos el resultado para hacer comparativa.

Cambiamos la pass de root a te56flon y apagamos la maquina.

Damos paso a HW
Cuando nos vuelva, hacemos toda la revision de nuevo:
sacamos otro crqcheck
listmaos los crqcheck con crqcheck --list
comparamos los dos ultimos con crqcheck -diff
revisamos el servicio de hora ntp/chrony

systemctl status ntpd
ntpstat

Cuando comprobemos que este todo correcto cambiamos de nuevo la pass de root a la antigua



usermod -p 'hash' root
