(TAS de validacion del parcheo)
Revisar a traves de la RSA: 

- System status sea correcto
- Que no haya Active events
- Que en el event log no haya nada de al menos 6 meses de antiguedad
- Que haya acceso por consola

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

Entrar por ssh a la maquina y realizar:

Nos creamos un fichero en nuestro home que se llame mkrear con el siguiente contenido de la wiki:

https://10.113.57.21:1521/bin/download/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/AUTOMATISMOS/Configuraci%C3%B3n%20y%20ejecuci%C3%B3n%20de%20backup%20para%20ReaR%20%5BSANTIAGO%5D/WebHome/mkrear.v8

Lanzamos luego:

source mkrear

y luego:

mkrear

Comprobamos que no haya ningun warning y si los hay los solucionamos hasta que no salga ninguno.

Despues lanzamos:

rear -d -v mkbackup

(Si el agente de trendmicro esta levantado puede fallar)

echo $?

que guardara un backup en la maquina AOTLXTEADM00003 en la ruta /rearbackup

Para recoger las evidencias hacemos:

ll
file backup.tar.gz rear-AOTLXPRPBD10055.iso

copiamos la salida y la adjuntamos al tratado de la tarea




