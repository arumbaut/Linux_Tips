Comandos Interesantes
```
show nos muestra informacion del contenedo en MetaSploit
#show exploits
#show payloads
#show auxiliar
```

Para hacer busqueda de info en la de los mudulos y demas
```
#search encoders
```

Viene integrado con nmap asi que dentro de el msfconsole podemos ejecutar comandos de nmap
```
nmap -sS -Pn
```

Escaners para joomla
```
use auxiliary/gather/joomla_weblink_sqli
```

Vulnerabilidades
buscamos auxiliares con comando search

Interactuar Nessus y MetaSploit
```
mfs>load nessus
mfs>nessus_connect admin:admin@ip:8834
mfs>nessus_policy_list
mfs>nessus_scan_new uuid Nombre "Descripcion" target
mfs>nessus_scan_launch scan_ id
```

Troyanos para Linux
con msfvenon  creamos el payload
search meterpreter   linux reverse tcp

Phishing

```
msf> searh http_basic
```

DinDNS que es buscar