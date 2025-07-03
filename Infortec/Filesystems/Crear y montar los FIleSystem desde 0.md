Ejemplo real nos pdicen que nos presentaron 20T formato de iscos de 5T para que creemos los filesystems corresmpondientes
Discos Presentados
```
60050763808105CB400000000000019F 
60050763808105CB40000000000001A0 
60050763808105CB40000000000001A1 
60050763808105CB40000000000001A2
```

Solucion 
Lo primero es verificar que el multipath es capaz de ver los discos que nos vienen en la peticion.

```
multipath -ll | grep -i -e 60050763808105CB400000000000019F -e 60050763808105CB40000000000001A0 -e 60050763808105CB40000000000001A1 -e 60050763808105CB40000000000001A2
```

Al no verlos haremos un reescan de los para que el multipath sea capaz de verlos

```
for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done
```

Reescaneamos para verificar que el multipath los detecto

```
root]# multipath -ll | grep -i -e 60050763808105CB400000000000019F -e 60050763808105CB40000000000001A0 -e 60050763808105CB40000000000001A1 -e 60050763808105CB40000000000001A2
mpathtx (360050763808105cb40000000000001a1) dm-566 IBM,2145
mpathty (360050763808105cb400000000000019f) dm-567 IBM,2145
mpathtz (360050763808105cb40000000000001a2) dm-568 IBM,2145
mpathua (360050763808105cb40000000000001a0) dm-569 IBM,2145
```

De aqui obtenemos los identificadores que les da multipath(mpathtx, mpathty, mpathtz, mpathua)

NOTA: 
**Verificamos lo VGs (Volume Group) para ver donde crearemos los PVs (Physical Volume) dentro de los VGs (Importante es los VGs van de 10 en 10 ejemplo TSMfile240_249) y dentro de estos ponemos los PVS  y aqui vamos poniendo los LVs que se crean de con notacion ascendente hasta llegar al 49 en este caso , iportante revisa para seguir el orden del lvm **
Ejemplos

```
[root@AOTLXPRTSM10008 root]# lvs
 LV                       VG             Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_TSMfile230            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile231            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile232            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile233            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile234            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile235            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile236            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile237            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile238            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile239            TSMfile230_239 -wi-ao----  <5.00t
  lv_TSMfile240            TSMfile240_249 -wi-ao----  <5.00t
  lv_TSMfile241            TSMfile240_249 -wi-ao----  <5.00t
  lv_TSMfile242            TSMfile240_249 -wi-ao----  <5.00t
  
```

Tambien podemos hacer un df -h para ver los filesystem creados con aterioridad y mas detalles como el punto de montaje.

```
[root@AOTLXPRTSM10008 root]# df -h
Filesystem                                  Size  Used Avail Use% Mounted on
/dev/mapper/TSMfile230_239-lv_TSMfile235    5.0T  5.0T     0 100% /tsmjz8/TSMfile235
/dev/mapper/TSMfile230_239-lv_TSMfile236    5.0T  5.0T     0 100% /tsmjz8/TSMfile236
/dev/mapper/TSMfile230_239-lv_TSMfile237    5.0T  1.7T  3.3T  35% /tsmjz8/TSMfile237
/dev/mapper/TSMfile230_239-lv_TSMfile238    5.0T  1.7T  3.3T  35% /tsmjz8/TSMfile238
/dev/mapper/TSMfile230_239-lv_TSMfile239    5.0T   28K  5.0T   1% /tsmjz8/TSMfile239
/dev/mapper/TSMfile240_249-lv_TSMfile240    5.0T   28K  5.0T   1% /tsmjz8/TSMfile240
/dev/mapper/TSMfile240_249-lv_TSMfile241    5.0T   28K  5.0T   1% /tsmjz8/TSMfile241
/dev/mapper/TSMfile240_249-lv_TSMfile242    5.0T   28K  5.0T   1% /tsmjz8/TSMfile242
```

Observar los PV que componen el LV.  Esta es la mejor opcion.
```
#lvs --segments -o +devices vg_name

[root@AOTLXPRTSM10008 ~]# lvs --segments -o +devices TSMfile240_249
  LV            VG             Attr       #Str Type   SSize  Devices
  lv_TSMfile240 TSMfile240_249 -wi-ao----    1 linear <5.00t /dev/mapper/mpathty(0)
  lv_TSMfile241 TSMfile240_249 -wi-ao----    1 linear <5.00t /dev/mapper/mpathtz(0)
  lv_TSMfile242 TSMfile240_249 -wi-ao----    1 linear <5.00t /dev/mapper/mpathua(0)
```

Partiendo de la nota anterios revisamos los VG disponibles y sus pv
```
vgs
```

Luego crearemos los PVs a partir de la informacion que ya tenemos
```
pvcreate /dev/mapper/mpathtx
#1750836142
pvcreate /dev/mapper/mpathty
#1750836148
pvcreate /dev/mapper/mpathtz
#1750836166
pvcreate /dev/mapper/mpathua
#1750836170
```

Si hay espacio en un VGs lo extenderemos de la siguiente manera
```
		 VGs a extender  Path que el multipath le dio al disco 
vgextend TSMfile230_239 /dev/mapper/mpathtx
```

Creamos el LV
```
			nombre lv                 VGs             Pv que utilizaremos
lvcreate -n lv_TSMfile239 -l 100%FREE TSMfile230_239 /dev/mapper/mpathtx
```

Le damos formato al filesystem
```
mkfs.ext4 -m0 /dev/mapper/TSMfile230_239-lv_TSMfile239
```

Creamos el punto de montaje
```
mkdir /tsmjz8/TSMfile239
```

Actualizamos el fstab con el nuevo filestem
```
vi /etc/fstab
```

Lo montamos 
```
mount /tsmjz8/TSMfile239
```

Como ya agregamos el ultimo LVM al VGs devemos crear un nuevo VG con rango siguiente en este caso 240 - 249 y le agregamos uno de los PVs que creamos anteriormente.

```
vgcreate TSMfile240_249 /dev/mapper/mpathty
```

Luego de creado lo extendmos con los discos restantes.
```
vgextend TSMfile240_249 /dev/mapper/mpathtz

vgextend TSMfile240_249 /dev/mapper/mpathua
```

Creamos los LV para estos nuevos Discos
```
			Nombre                    VGs             PV que utilizaremos
lvcreate -n lv_TSMfile240 -l 100%FREE TSMfile240_249 /dev/mapper/mpathty
lvcreate -n lv_TSMfile241 -l 100%FREE TSMfile240_249 /dev/mapper/mpathtz
lvcreate -n lv_TSMfile242 -l 100%FREE TSMfile240_249 /dev/mapper/mpathua
```

Le damos formato a cada uno de los LVs
```
mkfs.ext4 -m0 /dev/mapper/TSMfile240_249-lv_TSMfile240

mkfs.ext4 -m0 /dev/mapper/TSMfile240_249-lv_TSMfile241

mkfs.ext4 -m0 /dev/mapper/TSMfile240_249-lv_TSMfile242
```

Creamos los puntos de montaje
```
mkdir /tsmjz8/TSMfile240

mkdir /tsmjz8/TSMfile242

mkdir /tsmjz8/TSMfile241
```

Modificar el fstab con la nueva informacion
```
Nombre de la peticion que solicito la creacion del FS
#WO0000001173515   
/dev/mapper/TSMfile230_239-lv_TSMfile239 /tsmjz8/TSMfile239 ext4 defaults,x-systemd.device-timeout=300 1 2
/dev/mapper/TSMfile240_249-lv_TSMfile240 /tsmjz8/TSMfile240 ext4 defaults,x-systemd.device-timeout=300 1 2
/dev/mapper/TSMfile240_249-lv_TSMfile241 /tsmjz8/TSMfile241 ext4 defaults,x-systemd.device-timeout=300 1 2
/dev/mapper/TSMfile240_249-lv_TSMfile242 /tsmjz8/TSMfile242 ext4 defaults,x-systemd.device-timeout=300 1 2
```

Montamos los puntos de montajes
```
mount /tsmjz8/TSMfile240

mount /tsmjz8/TSMfile241

mount /tsmjz8/TSMfile242
```

Para revisar que todo marcha bien ejecutamos un comando pvs y debe mostrar todo correctamente.