
desde terminal si tiras usu aparecen todos los usuarios
aotlxterep00001 -> usu

es la 6a columna del fichero /etc/passwd

cat /etc/passwd | cut  -d “:” -f5

BUSCAR UN EJEMPLO

NUNCA DEJAR ESPACIOS EN BLANCO

El caracter delimitador es la barra del 7: “/”

**1: PAIS:** 

- ES
- PT

**2: TIPO DE USUARIO**

- F: Funcional. Es EL usuario, en mayúsculas. El usuario que usa todo el depto con todos los permisos. Como 'root' pero de otros deptos. Es usado por todas las personas del depto. Todos los deptos. tienen uno. La pasword tiene que darse de alta en una web que se llama Cyberark (dedicada solo a usuarios funcionales) (ej: root, oracle, weblogic)
- S: Subsistema / Servicios: No se loga nadie con ellos. Son usuarios que rulan en aplicaciones ellos solos haciendo cosas. Son necesarios. No son propios del S.O. . Ej: Apache, SSHD
- C: Customer. Cliente. Es FranceTelecom, Orange, Jazztel, Oracle. Por esta info sabemos que su home no tiene un vg del sistema operativo, y su home es /homeuser/usuario o uno fuera del SO. Son los que te llegan por una WO00000xxxx
- E: External. Nuestro, de aqui de la ofi o un service manager.
- K: Kyndril. Nuestro, de aqui de la ofi o un service manager.
- Ejemplos:

**ES/F**

**PT/E**

**ES/S**

**ES/C**

**ES/E**

**ES/K**

los que estan subrayaos son los mas comunes con diferencia

**3: NUMERO DE EMPLEADO**

En F y en S no tienen un numero de empleado, asi que le ponemos el grupo intermedio (depto), que son estos: 

- **ES/F/*EAUTSM - byr**

**ES/F/*EAWEB - j2ee**

**ES/F/*EFDBAD - oracle**

**ES/F/*EESECT - cyberark**

**ES/F/*EAUPLA - planificacion**

**ES/F/*EAUSOP - operacion**

**ES/F/*EAUXOS - nosotros**

**ES/F/*EAUTIV - monitorizacion**

- Ejemplo real: ES/S/*EAUTSM/KYNDRYL/ (154341)

Los E y los K si tienen numero de empleado, ponemos ese, el numero de empleado es por ejemplo el mio: es27589e. Ejemplos;

- :ES/K/K04194/KYNDRYL/Alejandro Martinez-ES-000060
- :ES/E/Y9F2M9/KYNDRYL/JUAN CARLOS HERRANZ JIMENEZ (ROLE ES-000013)

Los C no ponemos nada, ponemos la empresa repetida. Ejemplo:

- ES/C/FTE/FTE/Usuario Sigma weblogica WO0000000904176 

**4: EMPRESA**

Hay mil, las mas importantes:

- Kyndril: (Para las K y las F)
- Contractor (Para las E)
- FranceTelecom (Para los C)
- Orange (Para los C)
- Otras: Oesia, 

**5: DESCRIPCION**

NOMBRE + APELLIDOS + (Opcional) ROL. 

DESCRIPCION (Si no sabemos, buscar ese mismo usuario en otra maquina, o un usuario con las mismas características en esa misma maquina)

PERSONA SOLICITANTE + GRUPO DE REMEDY + NUMERO DE PETICION (WO).

Ejemplo: **ES/C/FTE/FTE/nombre solicitante - grupo remedy -wo/task**

**6: EJEMPLOS/PLANTILLAS**

**ES/C/FTE/FTE/Usuario Soporte Weblogic Helio Aseg TAS000002201083** (le pongo lo del principio porque he buscado un usuario que se parecia mucho el nombre - la descripcion me la dan en la inc ya, le pongo esa + la TAS, no la WO, porque la tas es mas concreta)

**ES/C/FTE/FTE/Usuario para dejar extracción de numeración de Giros WO0000001007452**

*******************************************************************

**cuando queramos comprobar si es tema de SOCKS el siguiente comando desde nuestro portátil**

**nc -w 1 -v 158.98.41.237 1080 < /dev/null**

**si el verbose muestra Connected es que conectamos con el SOCKS (que es esa IP)**

**otra cosa es que luego el SOCKS no nos deja pasar a** **red de cliente****, que sería más raro (pero tema SOCKS es tema Kyndryl)**

**si queremos saber si es tema VPN, que lo único que hace es permitirnos llegar a** **red Infortec**

**podemos hacer un PING a un servidor de Infortec y ver si responde, por ejemplo el del Notes (mientras dure)**

**ping 172.16.200.12**

**si no responde a ping es que la VPN no funciona**