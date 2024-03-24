# Powershell 7

Abrir powershell como administrador.

1. Presiona `WIN+R`
2. Escribe `pwsh`
3. Presiona `CTRL+SHIFT+ENTER`

Imprimir texto por pantalla.

```ps
Write-Host "hola mundo"
```

#### Comandos

```ps
# Muestra todos los comandos
Get-Command

# Muestra los comandos que incluyan la palabra Process
Get-Command *Process

# Muestra los comandos de un módulo
Get-Command -Module PowerShellGet
```

#### Ayuda

```ps
Get-Help Get-Process
Get-Help Get-Process -Detailed
Get-Help Get-Process -Full
Get-Help Get-Process -Online

# Podemos actualizar los manuales con
Update-Help -UICulture en-US

# También podemos obtener información del lenguaje
Get-Help about*
Get-Help about_Variables
```

#### Propiedades

Powershell trabaja con objetos, no con flujos de texto como bash, por lo que cada comando que ejecutemos retornará un objeto con sus propiedades. Para conocer estas propiedades podemos ejecutar el comando `Get-Member`.

```ps
Get-Process | Get-Member
```

#### Alias

```ps
Set-Alias -Name adapters -Value Get-NetAdapter
Get-Alias *adapter*
```

#### *Módulos*

```ps
# Disponibles e importados
Get-Module

# Disponibles
Get-Module -ListAvailable

# Mostrar las rutas donde se buscan los módulos
$env:PSModulePath -split ";"

# Buscar un nuevo módulo
Find-Module *math*

# Conocer los comandos de un módulo no instalado
Find-Command -ModuleName psmath

# Instalar un módulo
Install-Module psmath

# Importar el módulo
Import-Module psmath
```

#### Seleccionar

```ps
# Seleccionar los 5 primeros
Get-ChildItem | Select-Object -first 5

# Seleccionar los 3 últimos
Get-Process | Select-Object -last 3

# Saltar algunos resultados. Este devuelve 3,4,5
1,2,3,4,5,6 | Select-Object -last 3 -skip 1
```

#### Ordenar

```ps
Get-ChildItem -Path C:\Users | Sort-Object -Property Name -Descending
```

#### Filtrar

```ps
# Primera forma
Get-Process | Where-Object -Property CPU -gt -Value 1

# Segunda forma (como -Property y -Value son posicionales, podemos hacerlo más breve)
Get-Process | Where-Object CPU -gt 1

# Otros ejemplos
Get-Process | Where-Object ProcessName -like win*
Get-Process | Where-Object ProcessName -in "pwsh","bash"

# Sintaxis avanzada
Get-Process | WhereObject -FilterScript {$PSItem.ProcessName -eq 'pwsh'}

# Generalmente $PSItem se escribe como $_
Get-Process | WhereObject -FilterScript {$_.ProcessName -eq 'pwsh'}

# Múltiples condiciones
Get-Process | WhereObject -FilterScript {$_.ProcessName -eq 'pwsh' -and $_.CPU -gt 1}
```

| Comparison               | Operator  | Case-Sensitive Operator |
| ------------------------ | --------- | ----------------------- |
| Equality                 | -eq       | -ceq                    |
| Inequality               | -ne       | -cne                    |
| Greater than             | -gt       | -cgt                    |
| Less than                | -lt       | -clt                    |
| Greater than or equal to | -ge       | -cge                    |
| Less than or equal to    | -le       | -cle                    |
| Wildcard equality        | -like     | -clike                  |
| Wildcard contains        | -contains |                         |
| Wildcard is contained    | -in       |                         |

#### Medir rendimiento

Podemos medir el tiempo que tarda en ejecutarse con comando con `Measure-Command { <comando> }`.

```ps
MeasureCommand {Get-Process | Sort-Object -Property CPU -Descending | Where-Object CPU -gt 1}
```
