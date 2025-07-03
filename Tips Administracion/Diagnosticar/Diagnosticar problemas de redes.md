## 🧠 Escenario: *La red no funciona como debería, o no tienes conexión*

### 🔍 Objetivo: *Diagnosticar qué parte de la red está fallando*

---

## 🧪 PASO 1: Verificar conectividad de red física y lógica

```
ip a
```
- ¿La interfaz tiene una dirección IP asignada?
- ¿Está **UP** o **DOWN**?

🔧 Si no tiene IP:

```
sudo dhclient
```

O para interfaces modernas (con `systemd`/Netplan):

```
nmcli device status
nmcli connection show
```

---

## 🧪 PASO 2: Verificar la puerta de enlace y rutas

```
ip route
```
- ¿Existe una ruta por defecto (`default via <gateway>`)?
- ¿Cuál es tu gateway?

Ejemplo de salida:

```
default via 192.168.1.1 dev eth0
```

---

## 🧪 PASO 3: Probar conectividad con el gateway

```
ping -c 4 192.168.1.1
```

Si **esto falla**, el problema está en tu red local (o en tu NIC, o en la configuración IP).

---

## 🧪 PASO 4: Probar resolución DNS

```
ping -c 2 google.com
```

Si esto **falla pero puedes hacer ping a 8.8.8.8**, entonces el problema es **DNS**.

Verifica tus resolvers:

```
cat /etc/resolv.conf
```

Reemplaza con DNS públicos si es necesario:

```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

---

## 🧪 PASO 5: Probar conectividad externa por IP

```
ping -c 4 8.8.8.8
```

Si esto falla y el gateway respondía, puede ser un problema de firewall, NAT o bloqueo externo.

---

## 🧪 PASO 6: Diagnóstico de rutas con `traceroute` (o `tracepath`)

```
sudo apt install traceroute -y
traceroute 8.8.8.8
```

Te muestra por dónde va el tráfico y dónde se pierde.

---

## 🧪 PASO 7: Escaneo de puertos o conectividad específica con `telnet` o `nc`

Por ejemplo, para ver si puedes llegar al puerto 80 de un servidor:

```
nc -zv google.com 80
```

---

## 🧪 PASO 8: Verificar reglas de firewall

```
sudo iptables -L -n -v
sudo ufw status verbose
```
- Asegúrate de que no estés bloqueando tráfico saliente o de ciertas interfaces.

---

## 🧪 PASO 9: Diagnóstico de interfaces con errores

```
ip -s link
```
- Mira si hay **errores**, **colisiones** o **paquetes descartados** en la interfaz.

---

## 🧪 PASO 10: Mirar los logs del sistema

```
dmesg | grep -i eth
journalctl -u NetworkManager
```

Te ayudará a detectar si hay problemas con los controladores de red, interfaces, o DHCP.

---

## 🧰 EXTRA: Herramientas útiles para diagnóstico profesional

- `mtr` – combinación de ping y traceroute
	```
	mtr google.com
	```
- `tcpdump` – captura de paquetes en la interfaz
	```
	sudo tcpdump -i eth0
	```
- `nmap` – escaneo de puertos y hosts
	```
	sudo nmap -sn 192.168.1.0/24
	```

---

## ✅ EJEMPLO DE DIAGNÓSTICO REAL

Supongamos que un servidor no se puede conectar a Internet. Haría esto:

1. `ip a` → ¿Hay IP?
2. `ip route` → ¿Hay ruta por defecto?
3. `ping 192.168.1.1` → ¿Hay gateway?
4. `ping 8.8.8.8` → ¿Hay salida a Internet por IP?
5. `ping google.com` → ¿DNS resuelve?
6. `cat /etc/resolv.conf` → ¿Hay servidores DNS válidos?
7. `traceroute 8.8.8.8` → ¿Dónde se corta?
8. `iptables -L` → ¿Hay alguna política DROP?
9. `tcpdump -i eth0` → ¿Está saliendo tráfico?
10. `journalctl` → ¿Errores de red en el sistema?