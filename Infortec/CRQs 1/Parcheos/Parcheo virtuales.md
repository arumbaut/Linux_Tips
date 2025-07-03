(TAS de validacion del parcheo)

Acceder por ssh a la maquina, lanzar y revisar que no de error:

crqcheck

Acceder por ssh a la maquina, lanzar y guardar los datos en un TXT:

{
cat /etc/selinux/config | grep SELINUX= | grep -v "# SELINUX"
df -hPT /boot
df -hPT /usr
df -hPT /var
ip a
route -n
cat /etc/redhat-release
uname -a
cat /etc/shadow | grep root | cut -d ':' -f1-2 
}

(Tas de Revision del backup)

Se revisa si el equipo de BYR ha podido realizarlo.
De no ser asi, tendremos que realizar una copia en frio del VMDK y VMX del sistema en el repositorio de linux durante el cambio, previamente indicando al gestor de que en la tarea de primer reinicio dejen parada la maquina y que nos creen una tarea en el Des principal para poder realizar la copia nosotros.