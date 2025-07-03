### 🎯 **Objetivo**

Evitar la degradación del sistema, pérdida de datos y caídas de servicios debido a la falta de espacio en disco, actuando de forma segura, ordenada y sostenible.

---

## 🧭 **FASE 1: Evaluación y diagnóstico**

### 1.1. **Monitorear uso de disco**

- Ejecutar:
	```
	df -hT /
	du -xhd1 / | sort -hr | head -20
	```
- Revisar discos montados, puntos críticos (`/var`, `/tmp`, `/home`, `/opt`, etc.).

### 1.2. **Identificar archivos grandes y directorios pesados**

- Comandos útiles:
	```
	find / -type f -size +500M -exec ls -lh {} \; | sort -k 5 -hr | head -20
	```
	o herramientas como `ncdu`, `baobab`, `WinDirStat` (Windows).

### 1.3. **Verificar logs o archivos temporales crecientes**

- Revisar `/var/log`, archivos `.gz`, `.old`, `.log`.
- Logs de servicios (Apache/Nginx, MySQL, aplicaciones, etc.).

---

## 🧼 **FASE 2: Liberación inmediata de espacio**

### 2.1. **Rotación y limpieza de logs**

- Forzar rotación:
	```
	sudo logrotate -f /etc/logrotate.conf
	```
- Limpiar logs antiguos:
	```
	sudo journalctl --vacuum-time=7d
	sudo rm -rf /var/log/*.gz /var/log/*.1 /var/log/*-????????  # según política
	```

### 2.2. **Eliminar archivos temporales**

```
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
```

### 2.3. **Eliminar caché innecesaria**

- Limpieza de `apt`:
	```
	sudo apt clean
	sudo apt autoremove
	```
- Limpieza de paquetes no utilizados (`yum`, `dnf`, `pacman`, según distro).

### 2.4. **Mover archivos grandes a almacenamiento externo o secundario**

- Uso de NFS, discos adicionales (`/mnt/storage`), cloud, etc.

---

## 📦 **FASE 3: Medidas estructurales a mediano plazo**

### 3.1. **Ampliar particiones o agregar discos**

- En VM: expandir disco desde hipervisor (VMware, Hyper-V, KVM).
- Usar `lsblk`, `fdisk`, `parted`, `lvextend` + `resize2fs` para crecer volúmenes.
- Agregar disco nuevo y montar como `/data`, `/var/lib/docker`, etc.

### 3.2. **Separar puntos críticos en particiones distintas**

- `/var`, `/tmp`, `/home`, `/opt`, `/data`
- Esto ayuda a aislar el crecimiento de datos.

### 3.3. **Implementar cuotas o límites**

- Cuotas por usuario o aplicación para evitar llenado no controlado.

### 3.4. **Programar limpieza automatizada**

- `cron` + scripts de rotación, eliminación, backups.
- Políticas: conservar solo los últimos 7/15/30 días de datos/logs.

---

## 🛡️ **FASE 4: Prevención y monitoreo proactivo**

### 4.1. **Implementar monitoreo y alertas**

- Herramientas: **Nagios**, **Zabbix**, **Prometheus**, **Netdata**, **Grafana**, scripts personalizados.
- Alertas por correo, Telegram, Slack, etc. cuando uso supera umbral (ej: 80%).

### 4.2. **Políticas de uso de disco**

- Establecer normas de almacenamiento para equipos de desarrollo, BI, datos, etc.
- Revisión periódica de carpetas de usuarios, backups, etc.

### 4.3. **Auditorías de uso trimestrales**

- Informes de espacio por servidor, por aplicación, por entorno.
- Control de crecimiento año a año.

---

## 🧾 **Resumen del plan de acción**

| Fase | Objetivo | Herramientas/Comandos clave |
| --- | --- | --- |
| Diagnóstico | Detectar problema | `df`, `du`, `find`, `ncdu` |
| Limpieza rápida | Recuperar espacio | `rm`, `logrotate`, `journalctl` |
| Solución estructural | Evitar repetición | `lvextend`, separar particiones, NFS |
| Prevención | Automatizar y alertar | `cron`, `Nagios`, `Prometheus` |