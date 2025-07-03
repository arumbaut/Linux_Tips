
ps -ef | grep pmon -> revisas las instancias de bbdd 
ps -ef | grep mrp --> donde este este proceso o procesos (si son varias intancias) sera la standby

# Ejemplo de creacion de FS y montaje para backup de infinidat

LUN que nos presentan:
4200 Gb	/infinidat_scoringp	AOTLXPRSCO10001 / AOTLXPRSCO10002

yec	742b0f000000ea90000000000a925cc	lun 29
tor	742b0f000000ea80000000000a915c0	lun 29

Descubrir la LUN en la maquina:

for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done


----------------------------------

/infinidat_scoringpp

vgcreate -s 1G vginfinidat_scoringpp /dev/mapper/mpathco
lvcreate -n infinidat_scoringpp -l +100%FREE vginfinidat_scoringpp
mkfs.ext4 -m0 /dev/mapper/vginfinidat_scoringpp-infinidat_scoringpp

Meter en el fstab y montar
/infinidat_scoringp
----------------------------------

/infinidat_scoringpby

vgcreate -s 1G vginfinidat_scoringpby /dev/mapper/mpathed
lvcreate -n infinidat_scoringpby -l +100%FREE vginfinidat_scoringpby
mkfs.ext4 -m0 /dev/mapper/vginfinidat_scoringpby-infinidat_scoringpby

Meter en el fstab y montar