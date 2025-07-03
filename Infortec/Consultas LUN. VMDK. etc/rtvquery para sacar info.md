
- **Buscar info por WWN de LUN:**
[root@AOTLXTEADM00001 ~]# rvtquery-legacy --datastore % | grep 600507640082830e700000000000008d  
CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2                  naa.600507640082830e700000000000008d                       VMFS  2096896       697428      1399468    66      aotlxprvin10155|aotlxprvin10151|aotlxprvin10153 

- **Buscar info por nombre de DS:**
[root@AOTLXTEADM00001 ~]# rvtquery-legacy --datastore CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2  
NAME                                                    ADDRESS                               TYPE  CAPACITY_MIB  IN_USE_MIB  FREE_MIB  FREE_%  HOSTS  
CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2  naa.600507640082830e700000000000008d  VMFS  2096896       697428      1399468   66      aotlxprvin10155|aotlxprvin10151|aotlxprvin10153  
You have new mail in /var/spool/mail/root  

- **Buscar vmdks que contiene un DS:**
[root@AOTLXTEADM00001 ~]# rvtquery-legacy --vmdk % | grep CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2  
AOTLXPPALT00005                              Hard_disk_1   174080        False  persistent              None          False  False    [CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2] AOTLXPPALT00005/AOTLXPPALT00005.vmdk  
AOTLXPPALT00006                              Hard_disk_1   174080        False  persistent              None          False  False    [CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2] AOTLXPPALT00006/AOTLXPPALT00006.vmdk  
AOTLXPPALT00007                              Hard_disk_1   174080        False  persistent              None          False  False    [CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2] AOTLXPPALT00007/AOTLXPPALT00007.vmdk  
AOTLXPPALT00008                              Hard_disk_1   174080        False  persistent              None          False  False    [CLVC-JAZZTEL01_PRE_YEC_SVC_V7000_No_Comprimido_Datos_2] AOTLXPPALT00008/AOTLXPPALT00008_2.vmdk  

- **para ver si una maquina hace cluster con otra (gpfs, pcs, etc):**
[root@AOTLXTEADM00001 ~]# clusters --machine AOTLXPRFDT00001

Máquina: AOTLXPRFDT00001

Clusters a los que pertenece la máquina:

PACEMAKER(FDT_cluster)

Máquinas que pertenecen al mismo cluster:

Cluster: PACEMAKER(FDT_cluster)  
AOTLXPRFDT00001  
AOTLXPRFDT00002  
[root@AOTLXTEADM00001 ~]#
