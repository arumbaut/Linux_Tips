
---

Este procedimiento sustituye al obsoleto y tendrá que ser utilizado en máquinas donde se realice como primera configuración o donde ya se haya realizado.

Elegir la plantilla de configuración que se ajuste al tipo de interfaz disponible.

La plantilla de configuración permite crear una nueva conexión de red y no se pueden utilizar para modificar conexiones de red ya existentes.

Las líneas de configuración de 'gateway' sólo es necesario realizar sobre las conexiones que así lo tengan asignado y se tendrá que omitir en aquellas conexiones que no lo necesitan.

**Nota: para facilitar la identificación de las conexiones de red utilizaremos como nombre de la conexión 'name' el mismo valor que el nombre del interfaz 'device'.**

**El valor "ensXXX" se corresponden con el nombre del interfaz de red.**

**El valor "bondX" se corresponden con el nombre del interfaz de unión de varios interfaces de red.**

**El valor "VVV" se corresponde con el indentificador numérico de la VLAN.**

**El valor "H.H.H.H" se corresponde con la dirección IP que identifica al host.**

**El valor "MM" se corresponde con la versión decimal que idenfica la dirección IP de la máscara de red.**

**El valor "G.G.G.G" se corresponde con el gateway de red.**

---

(ETHERNET) (ACCESS PORT)

name="ensXXX"  
device="ensXXX"  
ipaddr="H.H.H.H/MM"  
gateway="G.G.G.G"  
nmcli connection add con-name ${name} ifname ${device} type ethernet  
nmcli connection modify ${name} ipv4.addresses ${ipaddr}  
nmcli connection modify ${name} ipv4.gateway ${gateway}  
nmcli connection modify ${name} ipv4.method manual

---

(ETHERNET) (TRUNK PORT)

name="ensXXX.VVV"  
device="ensXXX.VVV"  
parent="ensXXX"  
vlan="VVV"  
ipaddr="H.H.H.H/MM"  
gateway="G.G.G.G"  
nmcli connection add type vlan con-name ${name} ifname ${device} vlan.parent ${parent} vlan.id ${vlan}  
nmcli connection modify ${name} ipv4.addresses ${ipaddr}  
nmcli connection modify ${name} ipv4.gateway ${gateway}  
nmcli connection modify ${name} ipv4.method manual

---

(BOND ACTIVE-ACTIVE) (ACCESS PORT)

name="bondX"  
device="bondX"  
slave1="ensXXX"  
slave2="ensYYY"  
ipaddr="H.H.H.H/MM"  
gateway="G.G.G.G"  
nmcli connection add type bond con-name ${name} ifname ${device} bond.options "mode=802.3ad,miimon=100,lacp\_rate=1"  
nmcli connection add type ethernet slave-type bond con-name ${slave1} ifname ${slave1} master ${device}  
nmcli connection add type ethernet slave-type bond con-name ${slave2} ifname ${slave2} master ${device}  
nmcli connection modify ${name} ipv4.addresses ${ipaddr}  
nmcli connection modify ${name} ipv4.gateway ${gateway}  
nmcli connection modify ${name} ipv4.method manual

(BOND ACTIVE-BACKUP) (ACCESS PORT)

name="bondX"  
device="bondX"  
slave1="ensXXX"  
slave2="ensYYY"  
ipaddr="H.H.H.H/MM"  
gateway="G.G.G.G"  
nmcli connection add type bond con-name ${name} ifname ${device} bond.options "mode=active-backup,miimon=100"  
nmcli connection add type ethernet slave-type bond con-name ${slave1} ifname ${slave1} master ${device}  
nmcli connection add type ethernet slave-type bond con-name ${slave2} ifname ${slave2} master ${device}  
nmcli connection modify ${name} ipv4.addresses ${ipaddr}  
nmcli connection modify ${name} ipv4.gateway ${gateway}  
nmcli connection modify ${name} ipv4.method manual

---

(BOND ACTIVE-ACTIVE) (TRUNK PORT)

name="bondX"  
device="bondX"  
slave1="ensXXX"  
slave2="ensYYY"  
nmcli connection add type bond con-name ${name} ifname ${device} bond.options "mode=802.3ad,miimon=100,lacp\_rate=1"  
nmcli connection add type ethernet slave-type bond con-name ${slave1} ifname ${slave1} master ${device}  
nmcli connection add type ethernet slave-type bond con-name ${slave2} ifname ${slave2} master ${device}  
nmcli connection modify ${name} ipv4.method disabled ipv6.method disabled

(BOND ACTIVE-BACKUP) (TRUNK PORT)

name="bondX"  
device="bondX"  
slave1="ensXXX"  
slave2="ensYYY"  
nmcli connection add type bond con-name ${name} ifname ${device} bond.options "mode=active-backup,miimon=100"  
nmcli connection add type ethernet slave-type bond con-name ${slave1} ifname ${slave1} master ${device}  
nmcli connection add type ethernet slave-type bond con-name ${slave2} ifname ${slave2} master ${device}  
nmcli connection modify ${name} ipv4.method disabled ipv6.method disabled

(BOND+VLAN) (TRUNK PORT)

name="bondX.VVV"  
device="bondX.VVV"  
parent="bondX"  
vlan="VVV"  
ipaddr="H.H.H.H/MM"  
gateway="G.G.G.G"  
nmcli connection add type vlan con-name ${name} ifname ${device} vlan.parent ${parent} vlan.id ${vlan}  
nmcli connection modify ${name} ipv4.addresses ${ipaddr}  
nmcli connection modify ${name} ipv4.gateway ${gateway}  
nmcli connection modify ${name} ipv4.method manual

---

Comprobar que la configuración de red funciona y se ajusta a lo necesitado.

nmcli connection up ensXXX  
nmcli connection up ensXXX.VVV  
nmcli connection up bondX  
nmcli connection up bondX.VVV  
ip address show

---

FIN

- [Attachments (0)](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20%28NMCLI%29%20%5BSANTIAGO%5D/#Attachments)
- [History](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20%28NMCLI%29%20%5BSANTIAGO%5D/#History)
- [Information](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20%28NMCLI%29%20%5BSANTIAGO%5D/#Information)

No attachments for this page

## Welcome

Welcome to this XWiki!

## Recently Visited

- [Search](https://10.113.57.21:1521/bin/view/Main/Search)