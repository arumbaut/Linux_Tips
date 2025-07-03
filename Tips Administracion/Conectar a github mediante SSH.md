

### 4\. Alternativa recomendada: configurar SSH

Si querés evitar usar tokens, podés configurar acceso SSH:

- Generá una clave SSH con:
```
ssh-keygen -t ed25519 -C "tu_email@example.com"
```
- Copiá el contenido de la clave pública (`~/.ssh/id_ed25519.pub`) y pegala en GitHub en:

`Settings > SSH and GPG keys > New SSH key`

- Cambiá la URL remota de tu repo a SSH:
```
git remote set-url origin git@github.com:arumbaut/Linux_Tips.git
```
- Hacé push sin pedir usuario ni contraseña:
```
git push -u origin main
```