para quitar luns de media centers de byr:
 
aotlxprcmv10003                   
aotlxprcmv10005                   
aotlxprcmv10009                   
aotlxprcmv10001 
 
procedimiento paso a paso (ejemplo):
ver ultima fecha de reescaneo y usuario
```
grep -F 'for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done' /var/log/bashhist/root/sh_history.* | awk -F':' '{match($1,/sh_history\.([0-9]{8})_[^\.]*\.[^\.]*\.fr\.([a-z0-9]+)\.to\.root/,m); if(m[1]>=d){d=m[1];u=m[2]}} END{if(d&&u) print "Última fecha de reescaneo:",d,"Usuario:",u; else print "No se encontró reescaneo en historiales recientes."}'
```
 
multipath -ll | grep -i 3600507680c8184a0580000000000061e
multipath -ll mpathix
dmsetup info mpathix
multipath -f mpathix
echo 1 > /sys/block/sddo/device/delete
echo 1 > /sys/block/sdeo/device/delete
echo 1 > /sys/block/sddp/device/delete
echo 1 > /sys/block/sdep/device/delete

 
 
 
----
antes ver que el open count este a 0 (sin procesos abiertos del so sobre el disco)
[root@AOTLXPRCMV10009 ~]# dmsetup info mpathfy
Name:              mpathfy
State:             ACTIVE
Read Ahead:        256
Tables present:    LIVE
Open count:        0 <-----------------------------
Event number:      0
Major, minor:      253, 373
Number of targets: 1
UUID: mpath-3600507680c8184a05800000000000856