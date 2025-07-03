
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

3\. Conéctate sin contraseña

Ahora con:

```
ssh usuario@servidor
```

Me cree un scrip para copiar las authorized_key a las maquinas a las que quiero conectarme y despues con otro script utilizando un host bastion que tine mi authorize_key me conecto a las maquinas objetivos

Tambien puedes conectarte al servidor y popiarte en la direccionel fichero 
/authorized_keys  