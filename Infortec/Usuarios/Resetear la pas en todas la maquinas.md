Primero entramos a la maquina ADM00001 y nos vamos al directorio de Juancarlo 

```
cd /home/esy9f2m9/expect/chg_pass
```

Ejecutamos el fichero
```
./chg_pass
```

Este nos guiara en el proceso

Si solo queremos cambiar el pass en algunas maquinas especificas creamos un fichero y ponemos el nombre de las maquinas y ejecutamos el script pasando por parámetro el fichero que creamos 
```
./chg_pass maquinas
```

Si nos da problemas podemos ver el log de esta manera 

```
cat *AOTLXPRBPM00005*

```
```
grep -i AOTLXPRBPM00005 logtotal.20250605161247
```
