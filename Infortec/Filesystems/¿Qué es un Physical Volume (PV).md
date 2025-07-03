## ¿Qué es un Physical Volume (PV)?

Un **PV** es un **disco físico o partición** que ha sido inicializado por LVM y que puede ser utilizado como parte de un **Volume Group (VG)**.

### 🔄 Simplificando:

Un **PV** es como un ladrillo. Con varios ladrillos (PVs), construyes una pared (VG), y en esa pared puedes hacer ventanas (LVs = Logical Volumes).

---

## 📌 Tipos de dispositivos que pueden ser PVs:

- Un disco entero (ej: `/dev/sdb`)
- Una partición (ej: `/dev/sdb1`)
- Un volumen RAID o dispositivo virtual (ej: `/dev/md0`)

---

## 🔧 Cómo crear un PV:

1. **Verifica el disco** (por ejemplo, `/dev/sdb`):
	```
	lsblk
	```
2. **Inicializa el disco como PV:**
	```
	sudo pvcreate /dev/sdb
	```
3. **Verifica que se creó correctamente:**
	```
	sudo pvs
	```

---

## 📊 Comandos útiles:

| Comando | Descripción |
| --- | --- |
| `pvs` | Lista los Physical Volumes |
| `pvdisplay` | Muestra detalles de un PV |
| `pvcreate` | Inicializa un disco como PV |
| `pvremove` | Elimina la firma LVM de un PV |
| `pvresize` | Cambia el tamaño disponible en el PV |

---

## 🧩 Ejemplo de flujo LVM:

```
sudo pvcreate /dev/sdb
sudo vgcreate mi_vg /dev/sdb
sudo lvcreate -L 10G -n mi_lv mi_vg
sudo mkfs.ext4 /dev/mi_vg/mi_lv
sudo mount /dev/mi_vg/mi_lv /mnt
```