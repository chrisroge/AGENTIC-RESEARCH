```Powershell
# >>> Add portable Node + npm-global to USER PATH (PowerShell 5.1-safe)
$nodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
$npmPrefix = "$HOME\tools\npm-global-22"

# Backup current USER PATH (text file on Desktop)
$backup = "$HOME\Desktop\USER_PATH_BACKUP_$(Get-Date -Format 'yyyyMMdd-HHmmss').txt"
$current = [Environment]::GetEnvironmentVariable('Path','User')
Set-Content $backup ($current -as [string])

# Prepend our entries if not already present (dedupe, preserve order)
function Prepend-ToUserPath {
  param([string]$p)
  $parts = ($p -split ';') | Where-Object { $_ } | ForEach-Object { $_.Trim() }
  $want  = @($nodeHome, $npmPrefix)
  $norm = { param($x) ($x -replace '/', '\').TrimEnd('\').ToLowerInvariant() }

  # remove any existing matches
  $parts = $parts | Where-Object {
    $np = (& $norm $_)
    -not ($want | ForEach-Object { (& $norm $_) } | Where-Object { $np -eq $_ })
  }
  # prepend our desired entries
  ($want + $parts) -join ';'
}

$new = Prepend-ToUserPath ($current -as [string])
[Environment]::SetEnvironmentVariable('Path', $new, 'User')

Write-Host "User PATH updated. Fully close VS Code (File → Exit) and launch it again."
```


### PATH
We also need to add C:\Progra~1\Git\bin\ to the system PATH environment variable and it all works!
