## Sintaxis básica

bash

CopiarEditar

```
sort [opciones] archivo.txt
```

---

## 🧪 Ejemplo sencillo

Supongamos que tienes un archivo llamado `nombres.txt`:

nginx

CopiarEditar

`Carlos Ana Beatriz Daniel`

### Ordenar alfabéticamente:

bash

CopiarEditar

`sort nombres.txt`

📤 Salida:

nginx

CopiarEditar

`Ana Beatriz Carlos Daniel`

---

## 🔀 Usos comunes con archivos

| Acción                       | Comando                           |
| ---------------------------- | --------------------------------- |
| Orden ascendente             | `sort archivo.txt`                |
| Orden descendente            | `sort -r archivo.txt`             |
| Orden numérico               | `sort -n archivo.txt`             |
| Orden por columna específica | `sort -k 2 archivo.txt`           |
| Quitar duplicados ordenados  | `sort archivo.txt                 |
| Guardar el resultado         | `sort archivo.txt > ordenado.txt` |

---

## 📂 Con archivos con columnas (por ejemplo, CSV o TSV)

Si tienes un archivo `datos.csv` como:

CopiarEditar

`Juan,35 Ana,28 Luis,42`

Y quieres ordenarlo por la edad (segunda columna):

bash

CopiarEditar

`sort -t ',' -k2 -n datos.csv`

- `-t ','` → indica que el separador de campos es coma.
    
- `-k2` → usa la segunda columna.
    
- `-n` → orden numérico.
    

📤 Salida:

CopiarEditar

`Ana,28 Juan,35 Luis,42`