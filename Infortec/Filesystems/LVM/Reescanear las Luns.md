Si nos dan unas lun nuevas debemos escanear en caso de no tenerlas en el multipath
puedemos hacerlo mediante este comando

```
for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done
```

O exite un script pero que no esta en todas las maquinas
```
rescan-scsi-bus.sh
```

Normalmente se ponen 10 lun en los 