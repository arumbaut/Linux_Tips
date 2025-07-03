El comando:

```
ps -ef
```

es una forma común de listar **todos los procesos en ejecución** en un sistema Linux (incluyendo Red Hat, Ubuntu, WSL, etc.).

---

## 🧠 ¿Qué significa?

- `ps`: Muestra procesos activos en el sistema.
- `-e`: Lista **todos los procesos**, no solo los del usuario actual (equivale a `-A`).
- `-f`: Muestra una salida **en formato completo** (full format), con más columnas de información.

---

## 📋 Salida típica de `ps -ef`

```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:00 ?        00:00:02 /sbin/init
user      1234     1  0 10:05 pts/0    00:00:00 bash
user      2345  1234  0 10:06 pts/0    00:00:00 ssh usuario@host
```

### Columnas importantes:

| Columna | Significado |
| --- | --- |
| UID | Usuario que ejecuta el proceso |
| PID | ID del proceso |
| PPID | ID del proceso padre |
| C | Uso de CPU (no siempre relevante) |
| STIME | Hora en que comenzó el proceso |
| TTY | Terminal asociada al proceso (`?` = sin terminal) |
| TIME | Tiempo total de CPU usado |
| CMD | Comando usado para iniciar el proceso |

---

## 🔍 Ejemplos útiles

- **Ver todos los procesos de un usuario:**
	```
	ps -ef | grep adrian
	```
- **Buscar procesos relacionados con SSH:**
	```
	ps -ef | grep ssh
	```
- **Ver jerarquía de procesos más clara:**
	```
	ps -ef --forest
	```

```
ps aux | grep nombre_del_script
```

🔍 **Monitoreo de procesos**

- `ps aux` → Muestra todos los procesos en ejecución con detalles como usuario, CPU, memoria, etc.
- `ps -ef` → Similar a `ps aux`, pero con una estructura diferente y más información sobre los procesos padre e hijo.

🔎 **Filtrar procesos específicos**

- `ps aux | grep nombre_del_proceso` → Busca un proceso por nombre.
- `pgrep -af nombre_del_proceso` → Alternativa más limpia para encontrar procesos activos.

🛠 **Ver procesos de un usuario específico**

- `ps -u usuario` → Muestra solo los procesos de un usuario determinado.

🔄 **Ver procesos en árbol**

- `ps -ejH` → Muestra la jerarquía de procesos.

- `ps -axjf` → Otra forma de ver la relación entre procesos padre e hijo.
    
    🚫 **Detener procesos**
    
    - `kill PID` → Finaliza un proceso por su ID.
    - `kill -9 PID` → Fuerza la terminación del proceso.
    - `pkill -f nombre_del_proceso` → Mata procesos por nombre.
    
    📊 **Ver consumo de recursos**
    
    - `ps aux --sort=-%cpu` → Ordena por uso de CPU.
    - `ps aux --sort=-%mem` → Ordena por uso de memoria.