Nos conectamos a la maquina, fundamental a la hora de crear los usuarios hacerlo a partir de la maquina  AOTLXTEREP00001

Para buscar la maquina en caso de no recordar el nombre
```
[root@AOTLXTEADM00001 ~]# infoserver
[root@AOTLXTEADM00001 current]# ls -l | grep -i terep
```

```
ssh AOTLXTEREP00001
```

Esta es la maquina donde tenemos el script usu que se utiliza para verificar que usuarios deben estar en las maquinas. Una vez conectados ejecutamos el alis usu para ver los comandos que nos arroja

```
usu 
#USUARIOS OBLIGATORIOS

#Crear usuarios grupo ibmadmin

#USUARIOS BORRAR/FALTAN/GECOS

```

De aqui tenemos toda la informacion que son necesaria en las maquinas o por lo menos que deberiamos tener en las maquinas, le prestamos atencion al scrip debajo de la etiqueta \#USUARIOS BORRAR/FALTAN/GECOS pues al correrlo en la maquino objetivo nos dara la info de lo que actualmente esta  y lo que no en la PC que queremos modificar.

```
USUARIOS BORRAR
==============================

es083125:x:7910:809:ES/K/K00773/KYNDRYL/Hermosilla Beraza, Marco (UA884):/home/es083125:/bin/bash
esy9gj9h:x:10452:300:ES/E/Y9gj9h/INFORTEC/WOOLDER RUIZ GOMEZ ROLE ES-000013:/home/esy9gj9h:/bin/bash
esk02377:x:14169:816:ES/K/K02377/KYNDRYL/JENS JOHANNES ANDERSEN - ROLE ES-000032:/home/esk02377:/bin/bash
userdel es083125
userdel esy9gj9h
userdel esk02377

USUARIOS BASE FALTAN
==============================

es42499e
es48389e
mpolofue
esf27588
es82328
esy99j6a
esy9gpsv
ess01033
ess01047
esk03845
esk03946
esk03948
esk03952
esk04079
esk04227
esk03816
esk03875

USUARIOS EXCEPCION FALTAN
==============================

oracle
weblogic
tomj2ee
es081049
es081049
esy9bppd
esy9bppd
dfernanp
dfernanp
esy9ar02
esy99y3y
esy99y3x
esy9d5vc
esy9cp8x
esy9f36v
esy9g79r
esy9geu0

USUARIOS GECO MAL
==============================

En usuarios borrar nos dice los usuarios que debemos remover de la PC y en los usuarios base faltan estan todos los usuarios que faltan en esta PC estos los copiamos y nos lo llevamos  a la pc AOTLXTEREP00001 y lo ponemos en un fichero  para generar el script para crearnos con usu los comandos de creacion de usuarios que necesitamos crear 

```
# aqui copiamos los usuarios que faltan

```
vi usuario 
```

# esto nos devuelve los comandos de creacion de esos usuarios que faltan
```
usu | grep -f usuario 
```

# fijarnos bien en solo copiar el las lineas de useradd y usermod

Nos copiamos esa salida y la llevamos a la maquina donde queremos crear los users y nos creamos un fichero para ejecutarlo y revisamos que todo resulto bien.

Una vez terminado volvemos a pasar el scrip de revision para ver que ya no faltan los usuarios base que copiamos.

Nota los usuarios base son los que deben estar.