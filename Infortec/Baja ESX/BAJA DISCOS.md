Si no esta levantada la maquina entrar por RSA y levantarla.
Si no tiene habilitado el SSH entrar por la consola de la RSA y en troubleshoting habilitar el SSH

Nos conectamos al HOST por ssh y lanzamos los comandos:

Mirar discos que tiene:

esxcfg-mpath -L | awk '{print $3}'

Mirar puertos de fibra:

esxcli storage san fc list

Pasarle los datos a almacenamiento para que los despresenten:

grupo remedy: SPAIN ALMACENAMIENTO

cuando la devuelva almacenamiento comprobar que se han eliminado los discos con:

esxcfg-rescan -A