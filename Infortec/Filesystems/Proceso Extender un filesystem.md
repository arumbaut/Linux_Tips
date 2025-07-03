
Procedimiento para extender un filesystem
Empesaremos desde el procedimiento donde es necesario agregar un disco a la maquina virtual para poder agregarlos al file system este proceso se realiza en el ESXi

Luego de crear el disco en el servidor ESXi se lo asignaremos a una maquina que lo detectara como un dispositivo a que debemos crear como un [^2]physical volume (pvs), lo agregaremos al [^1]Volume Group (VG)

Partiendo de que no estara  creado el pv debido a que acabamos de agregar el disco lo crearemos con el siguiente comando. Siempre tener en cuenta que los nombres de las creaciones de los vgs o pvs nos deben venir en la solicitud de la tarea.

Para ver los discos tenemos varias opciones
```
lsblk

dmesg | tail -n 20
```

```
pvcreate /dev/mapper/sdk
# /dev/mapper/sdk es la ruta absoluta del nuevo dispositivo que agregamos a # la VM
```

Revisaremos los volumenes fisicos (pvs) en el servidor en cuestion para identificar la disponibilidad de espacio que tienen con el comando pvs. En este paso debemos ver nuestro pv ya creado.

```
[root@AOTLXPRPBD00025 ~]# pvs
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

Primero revisaremos si ya existe el VG listandolos
```
[root@AOTLXPRPBD00025 ~]# vgs
  VG              #PV #LV #SN Attr   VSize    VFree
  Commvault_vg      1   1   0 wz--n-  <10.00g       0
  datos_vg          1   7   0 wz--n-  <10.00g   <3.36g
  ister_vg          1   1   0 wz--n-   <2.00g 1020.00m
  mzos_vg           3   4   0 wz--n- <219.99g   25.99g
  oracle19_vg       1   2   0 wz--n-  <80.00g  316.00m
  oracle_ogi19_vg   1   1   0 wz--n-  <40.00g       0
  oracle_temp_vg    1   1   0 wz--n-  <20.00g  276.00m
  root_vg           1  11   0 wz--n-  <38.06g   <9.55g
  swap_vg           1   1   0 wz--n-  <20.00g       0
```

Ahora crearemos el VG nuevo para agregar nustro pv a el. 

```
vgcreate datosvg /dev/sdb /dev/sdc
```

En caso de existir el VG lo agregamos al existente con el siguiente comando

```
#vgextend nombre_vg_existente direccion_del_pv
vgextend mzos_vg /dev/sdk

```

Revisamos que se a agregado correctamente a nuestro vg el pv indicado
```
[root@AOTLXPRPBD00025 ~]# pvs
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
  ->/dev/sdk   mzos_vg         lvm2 a--   <20.00g  <20.00g
```

Y vemos que en efecto se agrego a mzos_vg se agrego /dev/sdk

Ahora vamos a extender el lvm indicado en la tarea. En este caso especifico le estamos indicando que tome 20Gigas y le estamos especificando de que pv especifico va a tomar a extender el espacio. Si no le indicamos /dev/sdd pues el solo decide de donde toma el espacio.

```
#lvextend -L +20G ruta_lv pv
lvextend -L +20G /dev/mapper/mzos_vg-lv_ora_mzospp01 /dev/sdd
```

Para usar todo el expacio libre disponible
```
lvextend -l +100%FREE /dev/datosvg/misdatos
```

Una ves extendido revisamos que realment extendio.
```
pvs
```

Luego haremos un redimensionamiento de archivos pues si ni lo hacemos el lv no se redimencionara aunque lo extendieramos antereiormente
```
resize2fs /dev/mapper/mzos_vg-lv_ora_mzospp01
```


Una ves extendido 
[^1]: Un **VG** es un contenedor lógico que agrupa uno o más **Physical Volumes (PVs)** —que suelen ser discos o particiones— y del cual puedes crear **Logical Volumes (LVs)** que se comportan como si fueran particiones normales.

[^2]: Un **PV** es un **disco físico o partición** que ha sido inicializado por LVM y que puede ser utilizado como parte de un **Volume Group (VG)**.
