## 1\. Generar un par de claves SSH (si no tienes ya)

Abre PowerShell y ejecuta:

```
ssh-keygen -t ed25519
```
- Presiona Enter para aceptar ubicación por defecto (`C:\Users\TuUsuario\.ssh\id_ed25519`)
- Puedes poner una passphrase para mayor seguridad o dejarla vacía para no pedir nada.

---

## 2\. Copiar la clave pública al servidor

Para que el servidor acepte tu clave y puedas conectar sin contraseña, necesitas poner tu clave pública en el servidor:

```
scp $env:USERPROFILE\.ssh\id_ed25519.pub usuario@servidor:/tmp/
```

Luego, conéctate (con contraseña esta vez) y ejecuta en el servidor:

```
mkdir -p ~/.ssh
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
rm /tmp/id_ed25519.pub
```

---

## 3\. Habilitar `PubkeyAuthentication yes` en el servidor (si tienes acceso)

Edita `/etc/ssh/sshd_config` y asegúrate de que tienes:

```
PubkeyAuthentication yes
```

Luego reinicia el servicio SSH:

```
sudo systemctl restart sshd
```

---

## 4\. Iniciar y configurar el agente SSH en Windows

En PowerShell (como administrador):

```
Start-Service ssh-agent
Set-Service -Name ssh-agent -StartupType Automatic
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

Esto cargará tu llave privada en el agente y solo te pedirá la passphrase (si la pusiste) una vez.

---

## 5\. Usar multiplexación para conexiones rápidas

Edita tu archivo de configuración SSH:

```
notepad $env:USERPROFILE\.ssh\config
```

Agrega:

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlPersist 600
```

Esto permitirá que las conexiones reutilicen una sesión abierta y no pidan contraseña repetidamente.

---

## 6\. Conéctate sin contraseña

Ahora con:

```
ssh usuario@servidor
```