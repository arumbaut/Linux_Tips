
Un **Logical Volume (LV)** es como una **partición flexible y dinámica** creada dentro de un **Volume Group (VG)**. Se comporta como una partición normal (puedes formatearlo, montarlo, usarlo), pero tiene ventajas clave:

---

## 🧱 Jerarquía LVM (resumen rápido):

```
[ Disco o partición ]  →  PV (Physical Volume)
                           ↓
                     VG (Volume Group)
                           ↓
                    LV (Logical Volume)
```

---

## ✅ Ventajas de usar LVs en lugar de particiones tradicionales:

| Ventaja | Explicación |
| --- | --- |
| Redimensionables | Puedes **ampliar o reducir** un LV sin reiniciar (con cuidado) |
| Agrupación de discos | Puedes unir varios discos en un solo VG y crear LVs sobre ese espacio total |
| Snapshots | Puedes crear **copias temporales (snapshots)** para backups |
| Flexibilidad | Los LVs no están limitados por los tamaños fijos de las particiones físicas |

---

## 🔧 Cómo se usa un LV (ejemplo):

### 1\. Crear el LV:

```
sudo lvcreate -L 10G -n mis_datos datosvg
```

Esto crea un LV de 10 GB llamado `mis_datos` dentro del VG `datosvg`.

### 2\. Formatearlo:

```
sudo mkfs.ext4 /dev/datosvg/mis_datos
```

### 3\. Montarlo:

```
sudo mount /dev/datosvg/mis_datos /mnt
```

---

## 🔍 Comandos útiles con LVs:

| Comando | Descripción |
| --- | --- |
| `lvs` | Lista los Logical Volumes |
| `lvdisplay` | Muestra detalles de un LV |
| `lvcreate` | Crea un nuevo LV |
| `lvextend` / `lvreduce` | Expande o reduce el tamaño de un LV |
| `lvremove` | Elimina un LV |