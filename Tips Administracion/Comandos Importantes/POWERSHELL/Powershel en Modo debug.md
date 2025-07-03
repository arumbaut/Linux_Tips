### Usar el depurador integrado (`Set-PSDebug`)

- Para activar el modo paso a paso:

powershell

CopiarEditar

`Set-PSDebug -Step`

Luego ejecutas tu script línea a línea, PowerShell te preguntará qué hacer (pasar a siguiente línea, entrar en función, salir, etc.).

- Para desactivar el modo debug:

`Set-PSDebug -Off`

---

### 2. Usar `Write-Host` o `Write-Output` para imprimir variables y mensajes

Ejemplo:
`Write-Host "Valor de variable x: $x"`

Esto es lo más sencillo y común para saber qué está pasando.

---

### 3. Usar `Breakpoints` con el ISE o Visual Studio Code

Si usas el editor **PowerShell ISE** o **VS Code** con extensión de PowerShell:

- Puedes poner breakpoints haciendo clic en la línea o con el comando `Set-PSBreakpoint`.
    
- Ejecutar el script en modo debug para detenerse en esos puntos.
    
- Inspeccionar variables, ejecutar comandos, etc.


Ejemplo para poner un breakpoint en la línea 10:

powershell

CopiarEditar

`Set-PSBreakpoint -Script "ruta\miScript.ps1" -Line 10`