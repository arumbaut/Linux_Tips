Este comando te devuelve el has del usuario que nos sirve para cambiarle las pass al usuario y despues restablecer la pas anterior.

read -p "Usuario:" user_mod ; echo "usermod -p \"$(grep "^${user_mod}:" /etc/shadow | cut -d: -f2 | sed 's/\$/\\$/g')\" ${user_mod}"

Solicita el nombre del usuario a configurar en este caso IVRVULA

Devuelve esta linea que es para reestablecer la pass 

usermod -p "\$1\$pk/WfHZF\$9ysFfGBjQxmWa/xCLvcOC/" IVRVULA