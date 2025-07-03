Para conectar a las instancias de cloud desde:

AOTLXTEADM00001.cosmos.es.ftgroup (10.113.46.179)    
sasg1.cosmos.es.ftgroup (10.113.46.215)    
sasg2.cosmos.es.ftgroup (10.113.46.169)


He dejado en /root de AOTLXTEADM00001 la key privada (id_rsa_cloud), podeis probar a conectar:

ssh -i id_rsa_cloud kynadm@10.152.0.15 ip del la maquina de gcp 

10.152.0.15 ce-wmir30ir-webmantened-aseg

Para las de OCI lo mismo pero:
ssh -i id_rsa_opc opc@

El equipo de RFS os pasara vuestras tareas una vez este desplegada la infra.

El usuario kynadm (escala a root, sudo su -) se eliminara una vez todos los grupos tecnicos terminen sus tareas, vosotros sois     
los primeros y meteis los usuarios, sudo (si aplica) o lo que consideis a nivel SO para ser autonomos sin el kynadm.

**CASO ESPECIAL DE CONEXION A GCP**

10.192.100.0/22 mm-infra-instances-pre    
10.192.104.0/22 mm-infra-instances-pro    
10.192.32.0/22 mm-md-infra-instances-pro    
10.192.48.0/22 mm-md-infra-instances-pre

Estas no podemos llegar desde ADM01 tenemos que utilizar 

El bastion gcplxteadm00001 de salto para llegar a ellas, usando kynadm y clave privadav