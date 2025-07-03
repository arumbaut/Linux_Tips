Al estar detras de un proxy soc5 y una VPN debemos revisar que wsl tenga conexion con internet y ademas con la red objetivo

All no tenerlo debemos utilizar el proxy de la institucion para esto instalamos proxychain

```
apt get upgrade
apt upgrade -y
apt install proxychains
nano /etc/proxychains.conf
```

agregamos la configuracion del proxy que utilizaremos 

```
socks5  IP   PORT
```

hacemos unas pruebas para ver si llegamos a la maquina bastion 

```
proxychains ssh user@x.x.x.x
```

al lograrlo nos crearemos un scrip para conectarno a las maquinas finales mediante el host bastion por ssh
Creamos un Directorio
Por convención, se suele usar `~/bin`:

```
mkdir -p ~/bin
```
Creamos un archivo

```
nano ~/bin/conect
```
Le agregamos lo siguiente

```
#!/bin/bash

destino="$1"

if [ -z "$destino" ]; then
    echo "Uso: conect <nombre_maquina>"
    exit 1
fi

usuarioGateway="user"
gateway="IP"
usuarioDestino="user"

# SSH vía proxychains
proxychains ssh -J "${usuarioGateway}@${gateway}" "${usuarioDestino}@${dest>
```

Hacemos el Script ejecutable
```
chmod +x ~/bin/conect
```


Nos aseguramos de que `~/bin` esté en nuestro  `$PATH`

```
nano ~/.bashrc
```

Agrega al final si no existe ya:

```
export PATH="$HOME/bin:$PATH"
```

Recargamos

```
source ~/.bashrc
```

Ejecuta el comando

```
conect 192.168.10.55
```


Para mas comodidad podemos agregar a nuestro host los resolvedores de dominios.
