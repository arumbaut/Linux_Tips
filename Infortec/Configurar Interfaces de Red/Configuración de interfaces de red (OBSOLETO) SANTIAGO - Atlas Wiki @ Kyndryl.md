
---

Este procedimiento se considera obsoleto por lo que sólo debe ser utilizado en máquinas donde ya exista una configuración previa y se necesite mantenerla.

Elegir la plantilla de configuración que se ajuste al tipo de interfaz disponible.

La plantilla de configuración permite crear una nueva conexión de red y no se pueden utilizar para modificar conexiones de red ya existentes.

Las líneas de configuración de 'gateway' sólo es necesario realizar sobre las conexiones que así lo tengan asignado y se tendrá que omitir en aquellas conexiones que no lo necesitan.

**Nota: para facilitar la identificación de las conexiones de red utilizaremos como nombre de la conexión 'name' el mismo valor que el nombre del interfaz 'device'.**

**El valor "ethX" se corresponden con el nombre del interfaz de red.**

**El valor "bondX" se corresponden con el nombre del interfaz de unión de varios interfaces de red.**

**El valor "VVV" se corresponde con el indentificador numérico de la VLAN.**

**El valor "H.H.H.H" se corresponde con la dirección IP que identifica al host.**

**El valor "M.M.M.M" se corresponde con la dirección IP que identifica la máscara de red.**

**El valor "G.G.G.G" se corresponde con la puerta de enlace de red.**

---

(ETHERNET) (ACCESS PORT)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-ethX'.

DEVICE=ethX  
IPADDR=H.H.H.H  
NETMASK=M.M.M.M  
GATEWAY=G.G.G.G  
ONBOOT=yes  
BOOTPROTO=static  
USERCTL=no

---

(ETHERNET) (TRUNK PORT)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-ethX.VVV'.

DEVICE=ethX.VVV  
IPADDR=H.H.H.H  
NETMASK=M.M.M.M  
GATEWAY=G.G.G.G  
ONBOOT=yes  
BOOTPROTO=static  
USERCTL=no  
VLAN=yes

---

(BOND+SLAVE)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-ethX'.

DEVICE=ethX  
NAME=bondX-slave  
ONBOOT=yes  
BOOTPROTO=none  
USERCTL=no  
TYPE=Ethernet  
MASTER=bondX  
SLAVE=yes

---

(BOND+MASTER) (ACCESS PORT)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-bondX'.

DEVICE=bondX  
IPADDR=H.H.H:H  
NETMASK=M.M.M.M  
GATEWAY=G.G.G.G  
ONBOOT=yes  
BOOTPROTO=static  
USERCTL=no  
BONDING\_OPTS="mode=802.3ad miimon=10 lacp\_rate=1"

---

(BOND+MASTER) (TRUNK PORT)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-bondX'.

DEVICE=bondX  
ONBOOT=yes  
BOOTPROTO=none  
USERCTL=no  
BONDING\_OPTS="mode=802.3ad miimon=10 lacp\_rate=1"

(BOND+VLAN) (TRUNK PORT)

Crear fichero de configuración de nombre '/etc/sysconfig/network-scripts/ifcfg-bondX.VVV'.

DEVICE=bondX.YYY  
IPADDR=H.H.H.H  
NETMASK=Y.Y.Y.Y  
GATEWAY=G.G.G.G  
ONBOOT=yes  
BOOTPROTO=static  
USERCTL=no  
VLAN=yes

---

Comprobar que la configuración de red funciona y se ajusta a lo configurado.

ifup ethX  
ifup ethX.VVV  
ifup bondX  
ifup bondX.VVV  
ip address show

---

FIN

- [Attachments (0)](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20para%20RedHat%20%5BSANTIAGO%5D/#Attachments)
- [History](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20para%20RedHat%20%5BSANTIAGO%5D/#History)
- [Information](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20para%20RedHat%20%5BSANTIAGO%5D/#Information)

No attachments for this page

## Welcome

Welcome to this XWiki!

## Recently Visited

- [Configuración de interfaces de red (NMCLI) \[SANTIAGO\]](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20%28NMCLI%29%20%5BSANTIAGO%5D/)
- [Search](https://10.113.57.21:1521/bin/view/Main/Search)