
### Buscar archivos que contengan `clave` en el nombre


`find /ruta/a/buscar -type f -iname '*clave*`


Buscar permisos 444
```
sudo find -type f -perm -444  > 

sudo find -type d -perm -111  <<sudo find -type f -perm -444
