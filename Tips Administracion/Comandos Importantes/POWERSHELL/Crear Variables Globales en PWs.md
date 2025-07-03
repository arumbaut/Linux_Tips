### 1. Variables de entorno persistentes (a nivel de usuario o sistema)

Para que una variable de entorno se mantenga después de reiniciar, debes definirla en el sistema o en el perfil de usuario.

#### Maneras de definir variables persistentes:

- **Desde el sistema (Windows GUI):**
    
    1. Abre "Configuración avanzada del sistema" → pestaña "Opciones avanzadas" → botón "Variables de entorno".
        
    2. Añades o editas variables para usuario o sistema.
        
    3. Estas variables estarán disponibles en futuras sesiones (después de abrir nuevas ventanas).
        
- **Desde PowerShell (definir variable de entorno permanente para usuario):**
    
        
    `[Environment]::SetEnvironmentVariable("MI_VAR", "valor", "User")`
    
    O para sistema (requiere permisos administrativos):
        
    `[Environment]::SetEnvironmentVariable("MI_VAR", "valor", "Machine")`
    
### 3. Para que los cambios surtan efecto

- Debes abrir una nueva sesión de PowerShell o consola para que tome la variable definida.
    
- No afectan a las sesiones ya abiertas.
    


## Resumen rápido

|Tipo|Se pierde al cerrar|Persiste tras reiniciar?|
|---|---|---|
|`$env:VAR = valor`|Sí|No|
|`[Environment]::SetEnvironmentVariable("VAR", "valor", "User")`|No|Sí|
|Variables GUI en sistema o usuario|No|Sí|

---

## Paso 1: Crear variables de entorno persistentes

Por ejemplo, para crear variables para el usuario actual (persisten tras reiniciar):

powershell

CopiarEditar

`[Environment]::SetEnvironmentVariable("GATEWAY_USER", "usuario_gateway", "User") [Environment]::SetEnvironmentVariable("GATEWAY_IP", "IP_Origen", "User") [Environment]::SetEnvironmentVariable("DESTINO_USER", "usuario_destino", "User") [Environment]::SetEnvironmentVariable("DESTINO_IP", "IP_Destino", "User")`

_Ejecuta esto una sola vez en PowerShell._

---

## Paso 2: Usar las variables en tu script

En tu script PowerShell simplemente accede con `$env:VARIABLE`:

```
$gatewayUser = $env:GATEWAY_USER 
$gatewayIP = $env:GATEWAY_IP 
$destinoUser = $env:DESTINO_USER 
$destinoIP = $env:DESTINO_IP  
# Ejemplo de uso con ssh (si tienes ssh en Windows) ssh "$gatewayUser@$gatewayIP"  ssh "$destinoUser@$destinoIP"

```
---

## Nota importante

- Para que las variables estén disponibles, **abre una nueva sesión PowerShell después de crearlas**.
    
- Si ya tenías una sesión abierta cuando las creaste, no las verás hasta que abras una nueva.