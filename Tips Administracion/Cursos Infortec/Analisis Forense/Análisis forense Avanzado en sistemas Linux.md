LiME precisa de tres paquetes, (make, build-essential y linux-headers). Los instalamos mediante 
```
sudo apt-get install make build-essential linux-headers
```

Con la instalación de linux-headers será necesario conocer qué versión de kernel estamos usando. Podemos averiguarlo con la orden: 
```
uname –r
```