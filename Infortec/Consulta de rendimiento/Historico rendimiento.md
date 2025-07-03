En la maquina el historico de rendimientos se guarda en:
/var/log/sa

[root@AOTLXPRVPX00026 sa]# cat /etc/crontab   
SHELL=/bin/bash  
PATH=/sbin:/bin:/usr/sbin:/usr/bin  
MAILTO=root

#Script extrae_procesos  
*/15 * * * * root /usr/local/bin/extrae_procesos  
55 23 * * *  root  /usr/sbin/logrotate -f /usr/local/bin/extrae_procesos.conf

[root@AOTLXPRVPX00026 sa]# pwd  
/var/log/sa  
[root@AOTLXPRVPX00026 sa]#