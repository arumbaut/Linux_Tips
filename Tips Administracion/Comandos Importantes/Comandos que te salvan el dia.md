Lista conexiones activas y los procesos que las usan
```
lsof -i
```

Muestra fechas exactas de modificacion
```
stat archivos.log
```

Ordenar directorios por tamaño 
```
du -sh * | sort -h
```

Detalla sesiones activas reinicios y entradas del sistema
```
who -a 
```

Muestra archivos inmutables. Ideal para detectar archivos protegidos o manipulados
```
lsattr
```

Verifica que esta siendo auditado por el sistema. Ideal para ambientes seguros
```
audictctl -l
```

Analisa puertos abiertos con procesos y direcciones IP conectadas
```
ss -apn
```