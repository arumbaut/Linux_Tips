Observar el nombre de dispositivo que representa el VG y LV que conforma el filesystem que se quiere eliminar. ^infofilesys

```
df -hPT MMMMM

[es48389e@AOTLXPRPBD00025 ~]$ df -hPT /dev/mapper/mzos_vg-lv_ora_mzospp01
Filesystem                          Type  Size  Used Avail Use% Mounted on
/dev/mapper/mzos_vg-lv_ora_mzospp01 ext4  132G   81G   46G  64% /ora_mzospp01
```

Si ponemos   df -hPT   vemos la informacion de todos 

```
df -hPT
```

Observar los PV que componen el LV.


```
#lvs --segments -o +devices vg_name

[root@maquina ~]#  lvs --segments -o +devices mzos_vg
  LV                   VG      Attr       #Str Type   SSize   Devices
  lv_ora_mzospp01      mzos_vg -wi-ao----    1 linear  64.00g /dev/sdd(2560)
  lv_ora_mzospp01      mzos_vg -wi-ao----    1 linear <50.00g /dev/sde(0)
  lv_ora_mzospp01      mzos_vg -wi-ao----    1 linear  20.00g /dev/sdd(31744)
  lv_ora_mzospp_arch01 mzos_vg -wi-ao----    1 linear  50.00g /dev/sdd(18944)
  lv_ora_mzospp_redo01 mzos_vg -wi-ao----    1 linear   5.00g /dev/sdd(1280)
  lv_ora_mzospp_redo02 mzos_vg -wi-ao----    1 linear   5.00g /dev/sdd(0)
```
- `lvs`  
    Lista los volúmenes lógicos (LVs).
    
- `--segments`  
    Muestra **cada segmento** de los LVs por separado (útil si un LV está distribuido entre varios discos o tiene diferentes tipos de almacenamiento, como striped o linear).
    
- `-o +devices`  
    Agrega la columna `devices`, que muestra los **dispositivos físicos (PVs)** utilizados en cada segmento.
    
- `vg_name`  
    Especifica que solo quieres ver los LVs que pertenecen al grupo de volúmenes llamado `vg_name`.

Observar el estado del PV a nivel de consumo de espacio y el nombre de dispositivo que lo representa.

```
[root]# pvs
  PV         VG              Fmt  Attr PSize    PFree
  /dev/sda2  root_vg         lvm2 a--   <38.06g   <9.55g
  /dev/sdb   swap_vg         lvm2 a--   <20.00g       0
  /dev/sdc   datos_vg        lvm2 a--   <10.00g   <3.36g
  /dev/sdd   mzos_vg         lvm2 a--  <150.00g   <6.00g
  /dev/sde   mzos_vg         lvm2 a--   <50.00g       0
  /dev/sdf   ister_vg        lvm2 a--    <2.00g 1020.00m
  /dev/sdg   oracle19_vg     lvm2 a--   <80.00g  316.00m
  /dev/sdh   oracle_temp_vg  lvm2 a--   <20.00g  276.00m
  /dev/sdi   oracle_ogi19_vg lvm2 a--   <40.00g       0
  /dev/sdj   Commvault_vg    lvm2 a--   <10.00g       0
  /dev/sdk   mzos_vg         lvm2 a--   <20.00g  <20.00g
```


Comprobar que el filesystem no está siendo utilizado por procesos activos del sistema.
```

#fuser -m direccion absoluta de la lv creada
[root@]# fuser -m /dev/mapper/mzos_vg-lv_ora_mzospp01
/dev/dm-5:           28087 28119 28123 28127 28153 28184 28188 28190 28194 28196 28198 28200 28325 28327 28329 28336 28350 28358 28360 28362 28364 28366 106015

```

Desmontar el filesystem, eliminar el punto de montaje y comentar su referencia en el fichero 'fstab'. [[#^infofilesys]]

```
#umount punto de montaje del donde esta el filesystem  
#rmdir punto de montaje del donde esta el filesystem
umount /ora_mzospp01
rmdir /ora_mzospp01

vi /etc/fstab
```

Desactivar y eliminar el LV.

```
lvchange -an lv_ora_mzospp01  
lvremove 
```

**Esta acción es destructiva por lo que es necesario confirmar que todos los pasos anteriores se ajustan a lo requerido.**

Observar el estado del PV que habrá cambiado a nivel de consumo de espacio y el nombre de dispositivo que lo representa.

```
pvs
```

**Si además de lo anterior necesitamos eliminar los discos que ya no estén siendo utilizados continuaremos con las siguientes acciones.**
[[Eliminar los discos que ya no estén siendo utilizados]]
