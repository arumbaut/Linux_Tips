## ¿Qué es un Volume Group (VG)?

Un **VG** es un contenedor lógico que agrupa uno o más **Physical Volumes (PVs)** —que suelen ser discos o particiones— y del cual puedes crear **Logical Volumes (LVs)** que se comportan como si fueran particiones normales.

---

## 🧱 Jerarquía de LVM:

```
Disco / Partición (sda1, sdb...) → PV (Physical Volume)
                ↓
             VG (Volume Group)
                ↓
           LV (Logical Volume)
```

---

## 🔹 Ejemplo práctico:

Supongamos que tienes dos discos `/dev/sdb` y `/dev/sdc`.

1. Los conviertes en Physical Volumes:
	```
	sudo pvcreate /dev/sdb /dev/sdc
	```
2. Creas un Volume Group llamado `datosvg`:
	```
	sudo vgcreate datosvg /dev/sdb /dev/sdc
	```
3. Luego, puedes crear Logical Volumes dentro del VG:
	```
	sudo lvcreate -L 10G -n mis_datos datosvg
	```
4. Y formatear/montar ese LV como si fuera una partición:
	```
	sudo mkfs.ext4 /dev/datosvg/mis_datos
	sudo mount /dev/datosvg/mis_datos /mnt
	```

---

## 🔍 Comandos útiles con VGs:

- Ver todos los VGs:
	```
	vgs
	```
- Ver detalles de un VG:
	```
	vgdisplay datosvg
	```
- Ver los LVs dentro de un VG:
	```
	lvs datosvg
	```