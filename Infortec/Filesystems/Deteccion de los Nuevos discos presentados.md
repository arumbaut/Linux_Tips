
Acceder por SSH al SO de la máquina.

(Máquina física)

Comprobar si es visible el identificador del disco asignado por el grupo de Almacenamiento. Para ver si el multipath ya fue capaz de mapear el disco presentado LLLL identificador del disco entregado por almacenamiento.

```
multipath -ll  
multipath -ll | grep -i LLLLL  
multipath -ll MMMMM
```

Si non vemos los discos procedemos a realizar un reescaneo de los discos
Realizar un rescaneo SCSI en todas las controladoras de discos.

```
for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done
```

Al terminar deberia porder ver los discos nuevos y para salir de dudas lo comprobamo.

```
multipath -ll  
multipath -ll | grep -i LLLLL  
multipath -ll MMMMM
```

Observar los nuevos nombres de dispositivo que se hayan creado en este instante de tiempo.

```
ls -lrt /dev/sd* /dev/xvd*
```

