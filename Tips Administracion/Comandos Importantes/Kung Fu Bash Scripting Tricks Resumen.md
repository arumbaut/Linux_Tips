Ejecutar ultimo comando lanzado como administrador
```
$ sudo !!
```

Revisar la hora
```
$ uptime

$ !u

$ sudo !w  # Referencia a whoami
```

Revisar el espacio de un directorio
```
$ df -hT /boot 

#Filesystem Type Size Used Avail Use% Mounted on
```

Repetir el ultimo comando que empieza con una cadena
```
$ !<String>
#Demostracios

adrian@DESKTOP-G5U9VQM:~$ who
adrian   pts/1        2025-06-26 14:08

adrian@DESKTOP-G5U9VQM:~$ w
 14:34:26 up 26 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
adrian   pts/1    -                14:08   26:21   0.02s  0.02s -bash

adrian@DESKTOP-G5U9VQM:~$ !w
w
 14:34:53 up 26 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
adrian   pts/1    -                14:08   26:48   0.02s  0.02s -bash

adrian@DESKTOP-G5U9VQM:~$ !wh
who
adrian   pts/1        2025-06-26 14:08
```

Manipulacion de procesos 
```
# Te da los procesos de un usuario
ps -U username

~$ ps -l
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S  1000    1329    1325  0  80   0 -  2578 do_wai pts/0    00:00:00 bash
0 R  1000    1438    1329  0  80   0 -  2635 -      pts/0    00:00:00 ps

$ ps aux

```

Reutilizar la segunda palabra (First Argument) del comando previo
```
$ host www.google.com 8.8.8.8
$ !^
www.google.com

$ ping -c1 !^
ping -c1 www.google.com
PING www.google.com (142.250.184.4) 56(84) bytes of data.
64 bytes from mad41s10-in-f4.1e100.net (142.250.184.4): icmp_seq=1 ttl=113 time=8.24 ms
```

Reutilizar la ultima palabra (Last Argument) del comando anterior  $ !$

```
$ unzip tpsreport.zip

$ rm !$ rm tpsreport.zip

$ mv cover-sheet.doc reports/ 

$ du -sh !$
```

Reference a Word of the Current Command and Reuse It $ !#:N

```
$ mv Working-with-Files.pdf Chapter-18-!#:1 

mv Working-with-Files.pdf Chapter-18-Working-with-Files.pdf
```

Save a Copy of Your Command Line Session $ script
```

```

Find out Which Commands You Use Most Often 
```
$ history | awk '{print $2}' | sort | uniq -c | sort -rn | head
```

Quitar comentarios y lineas en blanco
```
$ grep -E -v "^#|^$" file > file2
```

