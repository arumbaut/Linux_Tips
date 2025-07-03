## Migrar disco (pvmove) multipath (RAW) en Linux (proyectos de liberación de cabina) \[OLDZHAY\]


Situación:  
Caso: CRQ000000093963  
Disco antiguo (a liberar): /dev/mapper/mpathb o 20017380015ac0974  
Disco nuevo: LUN id: 3600507640082830e7000000000000130

Volumen actual (/dev/mapper/mpathb):  
\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathb datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
#  

\# multipath -ll  
mpathb (20017380015ac0974) dm-3 IBM,2810XIV  
size \= 16G features \= '0' hwhandler \= '0' wp \= rw  
\` -+- policy \= 'service-time 0' prio \= 1 status \= active  
|- 16:0:4:1 sdc 8:32 active ready running  
|- 16:0:5:1 sde 8:64 active ready running  
|- 18:0:1:1 sdb 8:16 active ready running  
\` - 18:0:0:1 sdd 8:48 active ready running  
#  

Escaneamos para detectar la nueva LUN (si no esta el rescan-scsi-bus.sh se instala: yum install sg3\_utils)

\# rescan-scsi-bus.sh -a  
Scanning SCSI subsystem for new devices  
Scanning host 0 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 1 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 2 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 3 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 4 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 5 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 6 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning for device 6 2 0 0...  
OLD: Host: scsi6 Channel: 02 Id: 00 Lun: 00  
Vendor: Lenovo Model: RAID 530 -8i Rev: 5.05  
Type: Direct-Access ANSI SCSI revision: 05  
Scanning host 7 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 8 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 9 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 10 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 11 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 12 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 13 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 14 for  SCSI target IDs   0 1 2 3 4 5 6 7, all LUNs  
Scanning host 15 for all SCSI target IDs, all LUNs  
Scanning host 16 for all SCSI target IDs, all LUNs  
NEW: Host: scsi16 Channel: 00 Id: 08 Lun: 10  
Vendor: IBM Model: 2145 Rev: 0000  
Type: Direct-Access ANSI SCSI revision: 06  
NEW: Host: scsi16 Channel: 00 Id: 09 Lun: 10  
Vendor: IBM Model: 2145 Rev: 0000  
Type: Direct-Access ANSI SCSI revision: 06  
Scanning for device 16 0 4 0...  
...  
...

Vemos que el disco se ha descubierdo:  
\# multipath -ll  
mpathb (20017380015ac0974) dm-3 IBM,2810XIV  
size \= 16G features \= '0' hwhandler \= '0' wp \= rw  
\` -+- policy \= 'service-time 0' prio \= 1 status \= active  
|- 16:0:4:1  sdc 8:32 active ready running  
|- 16:0:5:1  sde 8:64 active ready running  
|- 18:0:1:1  sdb 8:16 active ready running  
\` - 18:0:0:1  sdd 8:48 active ready running  
mpathu (3600507640082830e7000000000000130) dm-7 IBM,2145  
size \= 16G features \= '1 queue\_if\_no\_path' hwhandler \= '0' wp \= rw  
|-+- policy \= 'service-time 0' prio \= 50 status \= active  
| |- 16:0:9:10 sdg 8:96 active ready running  
| \` - 18:0:8:10 sdh 8:112 active ready running  
\` -+- policy \= 'service-time 0' prio \= 10 status \= enabled  
|- 16:0:8:10 sdf 8:80 active ready running  
\` - 18:0:9:10 sdi 8:128 active ready running  
#  
\# ll /dev/mapper/ |grep mpath  
lrwxrwxrwx 1 root root 7 Oct 26 13:34 mpathb ->../dm-3  
lrwxrwxrwx 1 root root 7 Oct 28 08:32 mpathu ->../dm-7  
#  

Ampliamos VG, migramos el dato y al terminar sacamos el PV antiguo:  
\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathb datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
\# vgextend datos\_vg /dev/mapper/mpathu  
Physical volume "/dev/mapper/mpathu" successfully created.  
Volume group "datos\_vg" successfully extended  
\# pvmove /dev/mapper/mpathb /dev/mapper/mpathu  
...  
\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathb datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/mapper/mpathu datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
#  
\# vgreduce datos\_vg /dev/mapper/mpathb  
Removed "/dev/mapper/mpathb" from volume group "datos\_vg"  
\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathb lvm2 --- 16.00g   16.00g  
/dev/mapper/mpathu datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
\# pvremove /dev/mapper/mpathb  
Labels on physical volume "/dev/mapper/mpathb" successfully wiped.  
\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathu datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
#  

Borramos el RAW antiguo:  
Vemos que el mpathb esta compuesto por los dispositivos sdb, sdc, sdd, sde. Los borramos:  
\# lsblk  
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT  
sda 8:0 0 557.9G   0 disk  
├─sda1 8:1 0 1M   0 part  
├─sda2 8:2 0 1G   0 part /boot  
└─sda3 8:3 0 556.9G   0 part  
├─root\_vg-lv\_root 253:0 0 2G   0 lvm /  
├─root\_vg-lv\_swap 253:1 0 128G   0 lvm \[SWAP\]  
├─root\_vg-lv\_usr 253:2 0 3G   0 lvm /usr  
├─root\_vg-lv\_tscm 253:6 0 512M   0 lvm /opt/IBM/SCM  
├─root\_vg-lv\_gestion 253:9 0 2G   0 lvm /gestion  
├─root\_vg-lv\_home 253:10 0 2G   0 lvm /home  
├─root\_vg-lv\_tmp 253:11 0 500M   0 lvm /tmp  
├─root\_vg-lv\_itm 253:12 0 2G   0 lvm /opt/IBM/ITM  
├─root\_vg-lv\_tivoli 253:13 0 1G   0 lvm /opt/Tivoli  
├─root\_vg-lv\_opt 253:14 0 2G   0 lvm /opt  
├─root\_vg-lv\_var 253:15 0 4G   0 lvm /var  
├─root\_vg-lv\_oracle 253:16 0 40G   0 lvm /oracle  
├─root\_vg-lv\_oracle\_ogi 253:17 0 30G   0 lvm /oracle\_ogi  
├─root\_vg-lv\_oracle\_temp 253:18 0 16G   0 lvm  
└─root\_vg-lv\_var\_opt\_ansible 253:19 0 2G   0 lvm /var/opt/ansible  
sdb 8:16 0 16G   0 disk  
└─mpathb 253:3 0 16G   0 mpath  
sdc 8:32 0 16G   0 disk  
└─mpathb 253:3 0 16G   0 mpath  
sdd 8:48 0 16G   0 disk  
└─mpathb 253:3 0 16G   0 mpath  
sde 8:64 0 16G   0 disk  
└─mpathb 253:3 0 16G   0 mpath  
sdf 8:80 0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdg 8:96 0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdh 8:112   0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdi 8:128   0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
#  
\# echo 1 > /sys/block/sdb/device/delete  
\# echo 1 > /sys/block/sdc/device/delete  
\# echo 1 > /sys/block/sdd/device/delete  
\# echo 1 > /sys/block/sde/device/delete  

...y comprobamos que el device ya no aparece:  
\# lsblk  
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT  
sda 8:0 0 557.9G   0 disk  
├─sda1 8:1 0 1M   0 part  
├─sda2 8:2 0 1G   0 part /boot  
└─sda3 8:3 0 556.9G   0 part  
├─root\_vg-lv\_root 253:0 0 2G   0 lvm /  
├─root\_vg-lv\_swap 253:1 0 128G   0 lvm \[SWAP\]  
├─root\_vg-lv\_usr 253:2 0 3G   0 lvm /usr  
├─root\_vg-lv\_tscm 253:6 0 512M   0 lvm /opt/IBM/SCM  
├─root\_vg-lv\_gestion 253:9 0 2G   0 lvm /gestion  
├─root\_vg-lv\_home 253:10 0 2G   0 lvm /home  
├─root\_vg-lv\_tmp 253:11 0 500M   0 lvm /tmp  
├─root\_vg-lv\_itm 253:12 0 2G   0 lvm /opt/IBM/ITM  
├─root\_vg-lv\_tivoli 253:13 0 1G   0 lvm /opt/Tivoli  
├─root\_vg-lv\_opt 253:14 0 2G   0 lvm /opt  
├─root\_vg-lv\_var 253:15 0 4G   0 lvm /var  
├─root\_vg-lv\_oracle 253:16 0 40G   0 lvm /oracle  
├─root\_vg-lv\_oracle\_ogi 253:17 0 30G   0 lvm /oracle\_ogi  
├─root\_vg-lv\_oracle\_temp 253:18 0 16G   0 lvm  
└─root\_vg-lv\_var\_opt\_ansible 253:19 0 2G   0 lvm /var/opt/ansible  
sdf 8:48 0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdg 8:96 0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdh 8:112   0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
sdi 8:128   0 16G   0 disk  
└─mpathu 253:7 0 16G   0 mpath  
├─datos\_vg-lv\_sigaux 253:4 0 100M   0 lvm /homeuser/sigaux  
├─datos\_vg-lv\_atoslec 253:5 0 500M   0 lvm /atoslec  
└─datos\_vg-lv\_proactiva 253:20 0 300M   0 lvm /proactivanet  
#  
\# multipath -ll  
mpathu (3600507640082830e7000000000000130) dm-7 IBM,2145  
size \= 16G features \= '1 queue\_if\_no\_path' hwhandler \= '0' wp \= rw  
|-+- policy \= 'service-time 0' prio \= 50 status \= active  
| |- 16:0:9:10 sdg 8:96 active ready running  
| \` - 18:0:8:10 sdh 8:112 active ready running  
\` -+- policy \= 'service-time 0' prio \= 10 status \= enabled  
|- 18:0:9:10 sdi 8:128 active ready running  
\` - 16:0:8:10 sdd 8:48 active ready running  
#  

Comprobamos que todo ha quedado bien:

\# pvs  
PV VG Fmt Attr PSize PFree  
/dev/mapper/mpathu datos\_vg lvm2 a-- < 16.00g < 15.12g  
/dev/sda3 root\_vg lvm2 a-- < 556.86g 321.84g  
\# lvs |grep datos  
 lv\_atoslec datos\_vg -wi-ao---- 500.00m  
lv\_proactiva datos\_vg -wi-ao---- 300.00m  
lv\_sigaux datos\_vg -wi-ao---- 100.00m  
\[root@AOTLXPREXA10001 ~\] \# df -hP |grep 'lv\_atoslec\\|lv\_proactiva\\|lv\_sigaux'  
/dev/mapper/datos\_vg-lv\_sigaux 93M 30M 56M 35 % /homeuser/sigaux  
/dev/mapper/datos\_vg-lv\_atoslec 477M 2.3M  445M 1 % /atoslec  
/dev/mapper/datos\_vg-lv\_proactiva 283M 103M 161M 39 % /proactivanet  
\[root@AOTLXPREXA10001 ~\] #  

Por ultimo, avisamos a storage que se lleve su LUN antigua

- [Attachments (0)](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/TUTORIALES/%5BOLDZHAY%5D%20Migrar%20disco%20%28pvmove%29%20multipath%20%28RAW%29%20en%20Linux%20%28proyectos%20de%20liberaci%C3%B3n%20de%20cabina%29/#Attachments)
- [History](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/TUTORIALES/%5BOLDZHAY%5D%20Migrar%20disco%20%28pvmove%29%20multipath%20%28RAW%29%20en%20Linux%20%28proyectos%20de%20liberaci%C3%B3n%20de%20cabina%29/#History)
- [Information](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/TUTORIALES/%5BOLDZHAY%5D%20Migrar%20disco%20%28pvmove%29%20multipath%20%28RAW%29%20en%20Linux%20%28proyectos%20de%20liberaci%C3%B3n%20de%20cabina%29/#Information)

No attachments for this page

## Welcome

Welcome to this XWiki!