Script PowerShell llamado `conect.ps1`

```
param (
    [string]$Destino
)

if (-not $Destino) {
    Write-Host "Uso: conect.ps1 <nombre_maquina_destino>"
    exit 1
}

```
# Ajusta estas variables
```
$usuarioGateway = "usuario_gateway"
$gateway = "gateway.miempresa.com"
$usuarioDestino = "usuario_destino"

```
# Construye el comando SSH con jump host
```
$sshCommand = "ssh -J $usuarioGateway@$gateway $usuarioDestino@$Destino"
```

Write-Host "Ejecutando: $sshCommand`n"
Invoke-Expression $sshCommand


## ¿Cómo usarlo?

1. Guarda ese archivo como `conect.ps1` en una carpeta, por ejemplo: `C:\Scripts`
    
2. Ejecuta desde PowerShell:



## Opción más rápida: Alias en PowerShell

Puedes definir un alias simple en tu perfil (`$PROFILE`) para tener el comando `conect` global:

### Paso 1: Crear el archivo  PowerShell_profile.ps1 si no existe

En PowerShell, ejecuta esto:
```
if (!(Test-Path -Path $PROFILE)) {
    New-Item -Type File -Path $PROFILE -Force
}

```


Agrega eso a tu perfil:

notepad $PROFILE

```
function conect {
    param($destino)

    if (-not $destino) {
        Write-Host "Uso: conect <nombre_maquina>"
        return
    }
	
  #  Write-Host "Prueba de alias $Destino "
   
    $usuarioGateway = "es48389e"
    $gateway = "10.113.46.179"
    $usuarioDestino = "es48389e"

    ssh -J "$usuarioGateway@$gateway" "$usuarioDestino@$destino"
}

```

Guarda y recarga el perfil
. $PROFILE

 Luego en cada sesión de PowerShell, podrás hacer simplemente:

ps  -> conect maquina123

Para poner powershel en modo debug para debuguear el codigo

