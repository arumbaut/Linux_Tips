Siempre verificar el estado en que se encuentran los puntos de montajes de los FileSystems.

Nos solicitan crear un file system con lel espacio disponible en un vg determinado  en este caso el  datos_vg 

Revisamos la existencia del vg en la maquina y si tiene capacidad disponible
 vgs
  VG        #PV   #LV   #SN       Attr          VSize        VFree
  ==datos_vg    1       11        0            wz--n-      629.94g      9.4g==
  ister_vg      1        1         0            wz--n-      1.94g          960.00m
  oracle_vg   1         2        0            wz--n-       40.00g        0
  root_vg      1        10       0            wz--n-       39.84g        20.31g
  swap_vg     1        1         0            wz--n-       20.00g        0


Vemos que tiene la capacidad y procedemos a la creacion del directorio donde se solicito que se montara el nuevo File System

```
mkdir /BinariosCommvault
```

Nos creamos el lvm para asignar todo el espacion del vg que nos indicaron
```
lvcreate -n lv_BinariosCommvault -l 100%FREE datos_vg
#Esta opcion agrega todo el espacio libre en el vg: -l 100%FREE
#Para un tamaño especifico utilizamos opcion -L 50g
# -n Opcion de nombre
```

Verificamos que se creo correctamente

```
[root@SGRE20YR ~]# lvs
  LV                   VG        Attr       LSize   Pool Origin Data%  
  ->lv_BinariosCommvault datos_vg  -wi-ao----   9.44g
  lv_homeuser          datos_vg  -wi-ao----  64.00m
  lv_homeuser_sigaux   datos_vg  -wi-ao----  64.00m
  lv_opt_cfm           datos_vg  -wi-ao----  64.00m
  lv_opt_gesoss        datos_vg  -wi-ao----   1.00g
  lv_opt_soposs        datos_vg  -wi-ao----  20.00g
  lv_opt_sybase        datos_vg  -wi-ao----   2.00g
  lv_oraarchive        datos_vg  -wi-ao---- 137.00g
  lv_oradata           datos_vg  -wi-ao---- 380.00g
  lv_orasys            datos_vg  -wi-ao----  80.00g
  lv_proactiva         datos_vg  -wi-ao---- 320.00m
  lv_monauto           ister_vg  -wi-ao----   1.00g
  lv_oracle            oracle_vg -wi-ao----  20.00g
  lv_oracle_ogi        oracle_vg -wi-ao----  20.00g
  lv_gestion           root_vg   -wi-ao----   2.00g
  lv_home              root_vg   -wi-ao----   2.00g
  lv_itm               root_vg   -wi-ao----   2.00g
  lv_opt               root_vg   -wi-ao----   2.00g
  lv_root              root_vg   -wi-ao----   2.00g
  lv_tivoli            root_vg   -wi-ao----   1.00g
  lv_tmp               root_vg   -wi-ao----   1.00g
  lv_usr               root_vg   -wi-ao----   3.03g
  lv_var               root_vg   -wi-ao----   4.00g
  lv_var_opt_ansible   root_vg   -wi-ao---- 512.00m
  lv_swap              swap_vg   -wi-ao----  20.00g
```

Le damos Formatos ext4
```
mkfs.ext4 -m0 /dev/mapper/datos_vg-lv_BinariosCommvault

#Por defecto, cuando se crea un sistema de archivos ext4, el comando #`mkfs.ext4` reserva un 5% del espacio total** para (root)
# -m0  le dice a `mkfs.ext4` que **no reserve bloques** para el #superusuario. Esto libera ese 5% adicional para uso general.
```

Creamos una copia de el fstab para luego escribir en el el montaje automatico de la particion
```
cp /etc/fstab /etc/fstab.20250610
```

Lo editamos 
```
vi /etc/fstab

Afregamos al fichero 
#WO0000001168538 -> Nombre de la solicitud que pidio el cambio
#Que se monta y donde se monta
/dev/mapper/datos_vg-lv_BinariosCommvault /BinariosCommvault ext4 defaults 0 2
```

Montamos el lvm

```
mount /BinariosCommvault
```

Verificamos que se monto correctamente
```
[root@SGRE20YR ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/root_vg-lv_root
                      2.0G  725M  1.1G  40% /
tmpfs                  18G  3.8G   14G  22% /dev/shm
/dev/sda1             146M   89M   50M  65% /boot
/dev/mapper/root_vg-lv_gestion
                      2.0G  372M  1.5G  21% /gestion
/dev/mapper/root_vg-lv_home
                      2.0G  5.9M  1.9G   1% /home
->/dev/mapper/datos_vg-lv_BinariosCommvault
                      9.2G   22M  9.2G   1% /BinariosCommvault
```

Salimos de la maquina y cerraos la incidencia
