
### Cómo guardarlo invertido en un nuevo archivo?

```
tac archivo.txt > archivo_invertido.txt
```

Extraer de la line 20 al final del fichero 
```
sed -n '12,$p' list_todas > list_prueba
```

Extraer en un rango determinado

```
sed -n '12,25p' list_todas > list_prueba
```

### ✅ Casos comunes:

| Lo que quieres hacer                | Comando sed                  |
| ----------------------------------- | ---------------------------- |
| Mostrar solo la **primera línea**   | `sed -n '1p' archivo.txt`    |
| Mostrar de la **línea 1 a la 5**    | `sed -n '1,5p' archivo.txt`  |
| Mostrar de la **línea 10 al final** | `sed -n '10,$p' archivo.txt` |
| Mostrar **todo el archivo**         | `sed -n '1,$p' archivo.txt`  |

 Muestra solo las líneas que contienen "error"
```
sed -n '/error/p' archivo.txt   # Muestra solo las líneas que contienen "error"
```

### **Eliminar líneas**


| sed '3d' archivo.txt       |     | # Elimina la línea 3                   |
| -------------------------- | --- | -------------------------------------- |
| sed '5,10d' archivo.txt    |     | # Elimina líneas de la 5 a la 10       |
| sed '/^$/d' archivo.txt    |     | # Elimina líneas vacías                |
| sed '/error/d' archivo.txt |     | # Elimina líneas que contienen "error" |

Si quieres eliminar el carácter `>` **y también el espacio que pueda quedar junto a él**, puedes usar:

```
sed -i 's/ *>//g' lista_ahora
```
#### Si el `>` puede estar seguido de espacios y también los quieres borrar:

```
sed -i 's/> *//g' lista_ahora
```

#### Si puede tener espacios antes y después:

```
sed -i 's/ *>\s*//g' lista_ahora
```
### 🗑️ **2. Eliminar líneas**

|**Acción**|**Comando `sed`**|
|---|---|
|Eliminar línea 3|`sed '3d' archivo.txt`|
|Eliminar líneas 5 a 10|`sed '5,10d' archivo.txt`|
|Eliminar líneas vacías|`sed '/^$/d' archivo.txt`|
|Eliminar líneas que contienen "error"|`sed '/error/d' archivo.txt`|

---

### 🔄 **3. Reemplazar texto**

| **Acción**                                       | **Comando `sed`**                |
| ------------------------------------------------ | -------------------------------- |
| Reemplazar primera ocurrencia de "foo" por "bar" | `sed 's/foo/bar/' archivo.txt`   |
| Reemplazar todas las ocurrencias de "foo"        | `sed 's/foo/bar/g' archivo.txt`  |
| Reemplazar ignorando mayúsculas/minúsculas       | `sed 's/foo/bar/gi' archivo.txt` |
| Reemplazar rutas con `/` usando otro delimitador | `sed 's                          |

---

### 📝 **4. Editar archivo directamente (in-place)**

|**Acción**|**Comando `sed`**|
|---|---|
|Editar el archivo directamente|`sed -i 's/foo/bar/g' archivo.txt`|
|Editar y crear copia `.bak`|`sed -i.bak 's/foo/bar/g' archivo.txt`|

---

### ➕ **5. Insertar o añadir líneas**

|**Acción**|**Comando `sed`**|
|---|---|
|Insertar antes de la línea 2|`sed '2i Nueva línea antes' archivo.txt`|
|Añadir después de la línea 3|`sed '3a Nueva línea después' archivo.txt`|
|Insertar antes de una línea con patrón|`sed '/patrón/i Línea antes del patrón' archivo.txt`|
|Añadir después de una línea con patrón|`sed '/patrón/a Línea después del patrón' archivo.txt`|

---

### 🔤 **6. Cambiar mayúsculas/minúsculas (GNU sed)**

|**Acción**|**Comando `sed`**|
|---|---|
|Todo el texto a mayúsculas|`sed 's/.*/\U&/' archivo.txt`|
|Todo el texto a minúsculas|`sed 's/.*/\L&/' archivo.txt`|
|Capitalizar primera letra|`sed 's/.*/\u&/' archivo.txt`|

---

### ⚙️ **7. Expresiones regulares y múltiples comandos**

| **Acción**                                                  | **Comando `sed`**                          |
| ----------------------------------------------------------- | ------------------------------------------ |
| Mostrar líneas que empiecen con "error" y contengan números | `sed -n '/^error.*[0-9]/p' archivo.txt`    |
| Ejecutar múltiples comandos (borrar y reemplazar)           | `sed -e '1d' -e 's/foo/bar/g' archivo.txt` |