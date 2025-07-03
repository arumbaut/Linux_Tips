## 🧠 ¿Qué es LVM?

**LVM** es un sistema de gestión de volúmenes lógicos que te permite:

- Crear "volúmenes lógicos" en lugar de particiones fijas.
- Redimensionar discos sin reiniciar el sistema.
- Combinar varios discos físicos en un solo volumen.
- Crear "snapshots" para respaldos rápidos.
- Expandir el almacenamiento en caliente (sin apagar el sistema).

---

## 🧱 Conceptos básicos de LVM

| Componente | Qué es |
| --- | --- |
| **PV** (Physical Volume) | Disco físico o partición preparada para LVM. |
| **VG** (Volume Group) | Grupo de volúmenes físicos, actúa como un "pool" de almacenamiento. |
| **LV** (Logical Volume) | Volumen lógico que se comporta como una partición o disco. |

### Ejemplo de jerarquía:

```
[ /dev/sda1 ] --> PV --> VG --> LV --> /home
```

---

## 🛠️ ¿Para qué se usa LVM?

- ✅ Expandir `/home` o `/var` fácilmente.
- ✅ Usar varios discos como si fueran uno solo.
- ✅ Hacer respaldos con snapshots.
- ✅ Redimensionar volúmenes sin reiniciar.
- ✅ Administrar almacenamiento dinámico en servidores y virtual machines.

---

## 🧪 Comandos básicos de LVM

### Crear un volumen LVM:

```
pvcreate /dev/sdb
vgcreate mis_datos_vg /dev/sdb
lvcreate -L 10G -n datos_lv mis_datos_vg
mkfs.ext4 /dev/mis_datos_vg/datos_lv
mount /dev/mis_datos_vg/datos_lv /mnt
```

### Ver estructura LVM:

```
pvs        # Ver physical volumes
vgs        # Ver volume groups
lvs        # Ver logical volumes
```

---

## 🔐 ¿Dónde se ve en Red Hat?

En Red Hat, cuando instalas el sistema, por defecto el instalador usa LVM para:

- `/` (root)
- `swap`
- `/home` (opcional)

Puedes ver esto con:

```
lsblk
```

o

```
vgs
lvs
```