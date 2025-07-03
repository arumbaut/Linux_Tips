info para la instalación de Trendmicro, en la versión de RedHat 8 hay un bug que si está el "secureboot" activado cuando se arranca el agente deja el servidor frito, hay que desactivar el "secureboot" para evitar problemas.
de esta manera nos ahorramos unos codiciados minutos de nuestro tiempo
`mokutil --sb-state`

Para rhel5 y rhel 6:  
Los paquetes están en -> /ks_boot/DS_AGENT/  

rhel 7 en adelante:  
Los scripts los tienes en  ->ks_boot/DS_AGENT/scripts

Para las activaciones  
  
proxy orange:  
/opt/ds_agent/dsa_control -r  
/opt/ds_agent/dsa_control -x dsm_proxy://10.132.14.200:8080/  
/opt/ds_agent/dsa_control -y relay_proxy://10.132.14.200:8080/  
/opt/ds_agent/dsa_control -a dsm://agents.workload.de-1.cloudone.trendmicro.com:443/ "tenantID:BF06D1BC-861C-49C8-021D-3F8C7941AF6E" "token:A4BEF4B1-7C86-8945-CEE3-3FF2DB709337" "policyid:332" "groupid:266"  

  
proxy jazztel:  
/opt/ds_agent/dsa_control -r  
/opt/ds_agent/dsa_control -x dsm_proxy://172.29.1.33:8080/  
/opt/ds_agent/dsa_control -y relay_proxy://172.29.1.33:8080/  
/opt/ds_agent/dsa_control -a dsm://agents.workload.de-1.cloudone.trendmicro.com:443/ "tenantID:BF06D1BC-861C-49C8-021D-3F8C7941AF6E" "token:A4BEF4B1-7C86-8945-CEE3-3FF2DB709337" "policyid:332" "groupid:266"  


DMZ sin proxy:  
/opt/ds_agent/dsa_control -r  
/opt/ds_agent/dsa_control -a dsm://agents.workload.de-1.cloudone.trendmicro.com:443/ "tenantID:BF06D1BC-861C-49C8-021D-3F8C7941AF6E" "token:A4BEF4B1-7C86-8945-CEE3-3FF2DB709337" "policyid:332" "groupid:266"

