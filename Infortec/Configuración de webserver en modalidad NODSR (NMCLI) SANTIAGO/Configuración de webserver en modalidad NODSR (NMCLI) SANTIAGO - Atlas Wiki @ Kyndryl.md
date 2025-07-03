---
title: "Configuración de webserver en modalidad NODSR (NMCLI) [SANTIAGO] - Atlas Wiki @ Kyndryl"
source: "https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20webserver%20en%20modalidad%20NODSR%20%28NMCLI%29%20%5BSANTIAGO%5D/"
author:
  - "[[SANTIAGO  TIRADO SALVADOR]]"
published:
created: 2025-06-03
description: "Configuración de webserver en modalidad NODSR (NMCLI) [SANTIAGO]"
tags:
  - "clippings"
---
## Configuración de webserver en modalidad NODSR (NMCLI) \[SANTIAGO\]

Last modified by [SANTIAGO TIRADO SALVADOR](https://10.113.57.21:1521/bin/view/XWiki/stirados) on 2024/03/26 15:51

---

Este procedimiento sustituye al obsoleto y tendrá que ser utilizado en máquinas donde NetworkManager se encuentre activo.

Acceder por SSH al SO.

Comprobar si es la primera vez que se realiza esta configuración.

ip route show table nodsr

**Nota: si el comando anterior devuelve un mensaje de error entonces continuar con el apartado de 'Primera configuración'.**

**Si el comando anterior no devuelve ningún mensaje de error entonces continuar con el apartado de 'Segunda y sucesivas configuraciones'.**

---

(Primera configuración)

Acceder al vCenter y localizar la VM.

Comprobar si existe una tarjeta de red conectada a la VLAN a la que pertence la IP de webserver que nos piden configurar y si no existe añadirla.

**Nota: si a nivel de ESX no está definida la VLAN será necesario solicitar al equipo de Redes la configuración de la misma en los switches de red.**

**Existe un procedimiento diferente sobre como obtener esta información.**

Acceder por SSH al SO y comprobar que la tarjeta de red es detectada.

ip link show

**Nota: podemos comparar la dirección MAC que aparece en el SO con la que aparece a nivel VMware y así confirmar el nombre del interfaz de red.**

Configurar la tabla de rutas para la modalidad NODSR.

echo -e "10\\tnodsr" >> /etc/iproute2/rt\_tables

Comprobar que se detecta la nueva tabla de rutas.

ip route show table nodsr

**Nota: si el comando anterior devuelve un mensaje de error entonces revisar los pasos realizados anteriormente.**

Comprobar si existe un perfil de conexión asociado con el nombre de interfaz de red identificado anteriormente.

nmcli connection show

**Nota: si existe entonces continuar con el apartado de 'Segunda y sucesivas configuraciones'.**

Configurar la dirección IP que identifica a la máquina en la VLAN de webserver con el procedimiento correspondiente para nuevos interfaces de red.

Configurar las opciones de Policy Routing.
``
name\_e="ensXXX"  
ipaddr="H.H.H.H/MM"  
netaddr="N.N.N.N/MM"  
gateway="G.G.G.G"  

\# RHEL7  
nmcli connection modify ${name\_e} +ipv4.routes "0.0.0.0/1 ${gateway} table=10"  
nmcli connection modify ${name\_e} +ipv4.routes "128.0.0.0/1 ${gateway} table=10"  
nmcli connection modify ${name\_e} +ipv4.routes "${netaddr} src=${ipaddr\%%/\*} table=10"  

# RHEL8 Y SUPERIORES  
nmcli connection modify ${name\_e} +ipv4.routes "0.0.0.0/0 ${gateway} table=10"  
nmcli connection modify ${name\_e} +ipv4.routes "${netaddr} src=${ipaddr%%/\*} table=10"

**Nota: esta dirección IP identifica a la propia máquina dentro de la VLAN por lo que no puede estar asociada a un webserver.**

**Si sólo nos han facilitado la dirección IP del webserver tendremos que solicitar a RFS una dirección IP adicional para asignar a la propia máquina.**

**No continuaremos con el resto de acciones hasta que nos hayan asignado esta nueva dirección IP para la propia máquina.**

**El valor 'ensXXX' se corresponde con el nuevo nombre de interfaz de red detectado anteriormente.**

**El valor 'H.H.H.H/MM' se corresponde con la dirección IP de la máquina y la máscara de red que la identifican dentro de la red de webserser.**

**El valor 'N.N.N.N/MM' se corresponde con la dirección IP de la red y la máscara de red que identifican a la propia red de webserver.**

**El valor 'G.G.G.G' se corresponde con la dirección IP del gateway correspondiente a la red de webserser.**

Levantar el perfil de conexión y comprobar que la dirección IP se ha activado.

nmcli connection up ensXXX  
ip address show

**Nota: el valor 'ensXXX' se corresponde con el nombre del perfil de conexión creado anteriormente.**

**Si el resultado de los comandos anteriores no muestra la configuración de red tendremos que revisar las acciones realizadas anteriormente.**

**Si queremos eliminar esta primera configuración de red ejecutaremos el comando 'nmcli connection delete ensXXX' donde el valor 'ensXXX' se corresponde con el nombre del perfil de conexión.**

Comprobar que existe una regla de Policy Routing que redirige el tráfico de esta nueva red de webserver a la tabla de rutas NODSR.

Comprobar que la tabla de rutas NODSR contiene una ruta para el gateway por defecto y otra ruta que define a toda la red de webserver apuntando contra la dirección IP del interfaz que se ha activado.

ip rule show  
ip route show table nodsr

Validar el resultado de todas las acciones anteriores.

**Nota: si el resultado de los comandos anteriores son correctos hemos completado la configuración de la dirección IP de la propia máquina en la VLAN.**

**Podemos continuar con 'Segunda y sucesivas configuraciones' para la configuración de la dirección IP del webserver.**

---

(Segunda y sucesivas configuraciones)

Acceder por SSH al SO e identificar el interfaz de red en el que se encuentra configurado el mismo direccionamiento IP que queremos añadir.

ip address

Comprobar que exista un perfil de conexión asociado con el nombre de interfaz de red identificado anteriormente.

nmcli connection show

Configurar la dirección IP como dirección secundaria en el perfil de conexión.

nmcli connection modify ensXXX +ipv4.addresses W.W.W.W/MM  
nmcli connection modify ensXXX +ipv4.routing-rules "priority 101 from W.W.W.W/32 table 10"

**Nota: esta dirección IP identifica al webserver que queremos configurar.**

**El valor 'ensXXX' se corresponde con el nombre de interfaz de red de la red de webserver detectado anteriormente.**

**El valor 'W.W.W.W/MM' se corresponde con la dirección IP del webserver y la máscara de red que lo identifican dentro de la red de webserser.**

**El valor 'W.W.W.W/32' se corresponde con la dirección IP del webserver y la máscara de red que sólo identifica a esta IP de webserser.**

Activar la nueva dirección IP sin reiniciar el perfil de conexión.

nmcli device reapply ensXXX

Comprobar que la dirección IP se encuentra activa como secundaria.

ip address show

**Nota: es necesario confirmar que la nueva dirección IP contiene la palabra 'secondary' en su línea de estado.**

**Si no contiene esta palabra revisar si la configuración de la máscara de red es la correcta y corregir.**

---

FIN

- [Attachments (0)](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20webserver%20en%20modalidad%20NODSR%20%28NMCLI%29%20%5BSANTIAGO%5D/#Attachments)
- [History](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20webserver%20en%20modalidad%20NODSR%20%28NMCLI%29%20%5BSANTIAGO%5D/#History)
- [Information](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20webserver%20en%20modalidad%20NODSR%20%28NMCLI%29%20%5BSANTIAGO%5D/#Information)

No attachments for this page

## Welcome

Welcome to this XWiki!