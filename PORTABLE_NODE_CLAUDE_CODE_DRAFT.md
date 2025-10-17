Downgrade Node to Long Term Support (LTS) version 22.20.0
Due to forward compatibility issues with the latest version of Node provided through Software Center some Claude Code users may experience errors when launching Claude Code. As a temporary work-around you may download and use the LTS version (22.20.0) with the following instructions:
1.	Download the LTS standalone zip from https://nodejs.org/dist/v22.20.0/node-v22.20.0-win-x64.zip 
2.	Create ‘tools’ folder within C:\Users\d-[YOUR.USER.NAME]
3.	Unzip to C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64

*Note: Once extracted, be sure that the file path reads as follows:
C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64

If your path reads:
C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64\node-v22.20.0-win-x64
all files are nested in an additional folder. Simply cut the nests folder and paste it into the ‘tools’ folder to remove the duplicate folder.

4.	Uninstall Node from Software Center
5.	Copy and paste the following commands in a Windows PowerShell terminal:
# === Portable Node + npm (no-admin) — CLEAN SETUP ===
# 0) Adjust these if you use a different Node folder/version:
$NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
$NpmPrefix = "$HOME\tools\npm-global-22"
$NpmCache  = "$HOME\tools\npm-cache-22"

# 1) Validate portable Node exists
if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) {
  Write-Error "node.exe not found at $NodeHome. Update `$NodeHome or unzip the Node win-x64 ZIP there."
  return
}

# 2) Ensure npm global + cache dirs exist (one-time)
New-Item -ItemType Directory -Force -Path $NpmPrefix, $NpmCache | Out-Null

# 3) Write a SAFE profile block (no expansion at write time)
$profilePath = $PROFILE   # e.g. ...\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
$backup = "$profilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
if (Test-Path $profilePath) { Copy-Item $profilePath $backup -Force }

$profileBlock = @'
# >>> portable-node (no-admin)
# NOTE: This block is written with a *single-quoted* here-string so variables expand only at shell startup.
$nodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
$npmPrefix = "$HOME\tools\npm-global-22"
$npmCache  = "$HOME\tools\npm-cache-22"

if (Test-Path (Join-Path $nodeHome 'node.exe')) {
  # Put portable Node first on PATH
  $env:Path = "$nodeHome;$env:Path"
  # Remove common hijackers (system Node and WindowsApps shims)
  $env:Path = ($env:Path -split ';' | Where-Object { $_ -and $_ -notmatch '\\Program Files\\nodejs|\\WindowsApps' }) -join ';'
}

# Prefer user npm "global" bin dir if present (Windows puts shims directly in prefix)
if (Test-Path $npmPrefix) {
  $env:Path = "$npmPrefix;$env:Path"
}
# <<< portable-node
'@

# Make sure profile folder exists, then write
$profileDir = Split-Path -Parent $profilePath
if (-not (Test-Path $profileDir)) { New-Item -ItemType Directory -Path $profileDir | Out-Null }
Set-Content -Path $profilePath -Value $profileBlock -Encoding UTF8

# 4) Apply the profile to *this* session
. $profilePath

# 5) One-time npm config to keep everything in your user folders (no admin)
# Invoke npm from the portable Node directly so we don't depend on PATH yet
$npmCmd = Join-Path $NodeHome 'npm.cmd'
if (Test-Path $npmCmd) {
  & $npmCmd config set prefix $NpmPrefix --location=user
  & $npmCmd config set cache  $NpmCache  --location=user
} else {
  # Fallback: call npm via node.exe + npm-cli.js
  $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js'
  & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user
  & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user
}

# 6) Verify
"Node locations (PATH order):"; where.exe node
"node -v:"; node -v
"npm -v:";  npm -v
"npm prefix:"; npm config get prefix
"npm cache:";  npm config get cache

"✅ Setup complete. Open a NEW PowerShell window; node/npm should be ready in every session."

# Make sure the new prefix is on PATH in this session (profile already handles future shells)
$env:Path = "$HOME\tools\npm-global-22;$env:Path"

# Install claude into the *current* npm prefix (C:\Users\<you>\tools\npm-global-22)
npm i -g @anthropic-ai/claude-code

# Verify the shim landed in the new prefix and resolves
where.exe claude
claude –version
# === end CLEAN SETUP ===


6.	OPTIONAL!  If you intend to use Claude Code from the official Claude Code VS Code extension copy and paste the following commands into your terminal, be sure to close and re-open VS Code afterwards:

# Fix for Claude Code VS Code Extension
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

# --- ALSO add Git\bin to SYSTEM PATH (requires admin) ---
$gitBin    = 'C:\Progra~1\Git\bin'
$machine   = [Environment]::GetEnvironmentVariable('Path','Machine')
try {
  if ($machine -notlike '*C:\Progra~1\Git\bin*') {
    [Environment]::SetEnvironmentVariable('Path', "$gitBin;$machine", 'Machine')
    Write-Host "System PATH updated with $gitBin. Fully close VS Code and reopen."
  } else {
    Write-Host "System PATH already contains $gitBin."
  }
} catch {
  Write-Warning "Could not modify System PATH (admin required). Run PowerShell as Administrator or add Git\bin to USER PATH instead."
}



*NOTE: VS Code Claude Code Extension Currently is Unavailable
Currently users are able to utilize Claude Code within Command Prompt/Terminal and VS Code Terminal.

With the combination of this new workaround for node.js and VS Code Extension updates, you may experience an error for the Claude Code UI Extension:

 

`Error: Claude Code requires Node.js version 18 or higher to be installed.`

We are currently troubleshooting a fix for this error, as the VS Code Extension cannot recognize the new node.js version we have installed.




Rollback script – Run to reverse these changes and no longer point to the portable Node solution
# === ROLLBACK portable Node/npm (user-level, no admin) — PS 5.1 SAFE ===
# Safety toggles (set to $true if you also want to delete folders on disk)
$RemovePortableNodeDirs = $false   # deletes $NodeHome if true
$RemoveNpmDirs          = $false   # deletes $NpmPrefix and $NpmCache if true
$RemoveNVS              = $false   # deletes %LOCALAPPDATA%\nvs if true

# Your paths (note: using lower-case 'tools')
$NodeHome    = Join-Path $HOME 'tools\node-v22.20.0-win-x64'
$NpmPrefix   = Join-Path $HOME 'tools\npm-global-22'
$NpmCache    = Join-Path $HOME 'tools\npm-cache-22'
$NvsHome     = Join-Path $env:LOCALAPPDATA 'nvs'
$WindowsApps = Join-Path $env:LOCALAPPDATA 'Microsoft\WindowsApps'
$AppDataNpm  = Join-Path $env:APPDATA     'npm'

Write-Host "==> Backing up and cleaning PowerShell profiles..."

function Remove-PortableBlockFromProfile {
  param([string]$ProfilePath)
  if (-not $ProfilePath) { return }
  $dir = Split-Path -Parent $ProfilePath
  if (-not (Test-Path $dir)) { return } # profile never created

  if (Test-Path $ProfilePath) {
    $backup = "$ProfilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
    Copy-Item $ProfilePath $backup -Force

    $raw = Get-Content $ProfilePath -Raw -ErrorAction SilentlyContinue
    if ($raw -match '# >>> portable-node') {
      $clean = [regex]::Replace($raw,'(?s)# >>> portable-node.*?# <<< portable-node\s*','')
      Set-Content -Path $ProfilePath -Value $clean -Encoding UTF8
      Write-Host "   - Removed portable-node block from $ProfilePath"
    } else {
      Write-Host "   - No portable-node block found in $ProfilePath (skipped)"
    }
  }
}

# Targets: Windows PowerShell + PowerShell 7 profile (if it exists)
$Profiles = @(
  $PROFILE,
  (Join-Path $HOME 'Documents\PowerShell\Microsoft.PowerShell_profile.ps1')
) | Select-Object -Unique

$Profiles | ForEach-Object { Remove-PortableBlockFromProfile $_ }

Write-Host "==> Restoring User PATH..."

function Remove-FromPath {
  param([string]$PathValue, [string[]]$RemoveStartsWith, [string]$EnsureContains)

  if ($null -eq $PathValue) { $PathValue = '' }
  $parts = $PathValue -split ';' | Where-Object { $_ -ne '' } | ForEach-Object { $_.Trim() }

  # Normalize for comparison
  $norm = { param($p) ($p -replace '/', '\').TrimEnd('\') }

  $patterns = @()
  foreach ($r in $RemoveStartsWith) {
    if ($r) { $patterns += (& $norm $r).ToLowerInvariant() }
  }

  $filtered = @()
  foreach ($p in $parts) {
    $np = (& $norm $p).ToLowerInvariant()
    $startsWithPattern = $false
    foreach ($pat in $patterns) {
      if ($pat -ne '' -and $np.StartsWith($pat)) { $startsWithPattern = $true; break }
    }
    if (-not $startsWithPattern) { $filtered += $p }
  }

  # Ensure WindowsApps is present (typical clean-user PATH)
  if ($EnsureContains -and -not ($filtered | Where-Object { $_.Trim().ToLowerInvariant() -eq ((& $norm $EnsureContains).ToLowerInvariant()) })) {
    $filtered = @($EnsureContains) + $filtered
  }

  # Deduplicate while preserving order
  $seen = @{}
  $deduped = @()
  foreach ($p in $filtered) {
    $key = ((& $norm $p).ToLowerInvariant())
    if (-not $seen.ContainsKey($key)) { $seen[$key] = $true; $deduped += $p }
  }
  ($deduped -join ';')
}

# Build removal list (covers both 'tools' and accidental 'Tools', plus NVS and npm shims)
$removeList = @(
  $NodeHome,
  $NpmPrefix,
  $NpmCache,
  (Join-Path $HOME 'tools\node-v'),   # any other portable node-v* under tools
  (Join-Path $HOME 'Tools\node-v'),   # cover accidental 'Tools'
  $NvsHome,
  $AppDataNpm
)

# Compute a new *User* PATH (if current is empty, we'll rebuild from scratch)
$userPath = [Environment]::GetEnvironmentVariable('Path','User')
if ($null -eq $userPath -or $userPath.Trim() -eq '') {
  $newUserPath = $WindowsApps
} else {
  $newUserPath = Remove-FromPath -PathValue $userPath -RemoveStartsWith $removeList -EnsureContains $WindowsApps
}

[Environment]::SetEnvironmentVariable('Path', $newUserPath, 'User')

# Update this session PATH (non-persistent)
$machinePath = [Environment]::GetEnvironmentVariable('Path','Machine')
if ($null -eq $machinePath) { $machinePath = '' }
$env:Path = ($newUserPath, $machinePath -join ';')

Write-Host "   - User PATH updated."

Write-Host "==> Clearing npm user config (prefix/cache) ..."

# Try npm if present; else edit ~/.npmrc
$gc = Get-Command npm -ErrorAction SilentlyContinue
$npmExe = $null
if ($gc) { $npmExe = $gc.Source }

if ($npmExe) {
  try { npm config delete prefix --location=user | Out-Null } catch { }
  try { npm config delete cache  --location=user | Out-Null } catch { }
}

# Also clean ~/.npmrc lines explicitly
$npmrc = Join-Path $HOME '.npmrc'
if (Test-Path $npmrc) {
  $backupNpmrc = "$npmrc.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
  Copy-Item $npmrc $backupNpmrc -Force

  $lines = Get-Content $npmrc -ErrorAction SilentlyContinue
  $kept  = $lines | Where-Object { $_ -notmatch '^\s*(prefix|cache)\s*=' }
  if ($kept.Count -eq 0) {
    Remove-Item $npmrc -Force
    Write-Host "   - Removed empty $npmrc"
  } else {
    Set-Content -Path $npmrc -Value $kept -Encoding UTF8
    Write-Host "   - Cleaned prefix/cache entries in $npmrc"
  }
}

Write-Host "==> Optional removals (set switches at top to `$true to enable)..."
if ($RemovePortableNodeDirs -and (Test-Path $NodeHome)) {
  Remove-Item -Recurse -Force $NodeHome
  Write-Host "   - Deleted $NodeHome"
}
if ($RemoveNpmDirs) {
  foreach ($d in @($NpmPrefix, $NpmCache)) {
    if ($d -and (Test-Path $d)) { Remove-Item -Recurse -Force $d; Write-Host "   - Deleted $d" }
  }
}
if ($RemoveNVS -and (Test-Path $NvsHome)) {
  Remove-Item -Recurse -Force $NvsHome
  Write-Host "   - Deleted $NvsHome"
}

Write-Host "==> Verification:"
Write-Host "   User PATH now:"
[Environment]::GetEnvironmentVariable('Path','User')
Write-Host "   where.exe node:"
where.exe node 2>$null | ForEach-Object { "     $_" }
Write-Host "   node -v (may be blank if none installed):"
try { node -v } catch { "     (node not on PATH in this session)" }

Write-Host "`n✅ Rollback complete. Open a NEW Windows PowerShell window."
# === end ROLLBACK ===


Repair script for anyone who ran earlier version of the above script which would have broken profile
# === REPAIR BROKEN PROFILE ===
$NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
$NpmPrefix = "$HOME\tools\npm-global-22"
$NpmCache  = "$HOME\tools\npm-cache-22"

if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) {
  Write-Error "node.exe not found at $NodeHome. Fix `$NodeHome first."
  return
}

# Targets: Windows PowerShell (shown in your error) and PowerShell 7 (if present)
$targets = @(
  $PROFILE,
  (Join-Path $HOME 'Documents\PowerShell\Microsoft.PowerShell_profile.ps1')
) | Select-Object -Unique

$safeBlock = @'
# >>> portable-node (no-admin)
$nodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
$npmPrefix = "$HOME\tools\npm-global-22"
$npmCache  = "$HOME\tools\npm-cache-22"

if (Test-Path (Join-Path $nodeHome 'node.exe')) {
  $env:Path = "$nodeHome;$env:Path"
  $env:Path = ($env:Path -split ';' | Where-Object { $_ -and $_ -notmatch '\\Program Files\\nodejs|\\WindowsApps' }) -join ';'
}
if (Test-Path $npmPrefix) {
  $env:Path = "$npmPrefix;$env:Path"
}
# <<< portable-node
'@

foreach ($p in $targets) {
  $dir = Split-Path -Parent $p
  if (-not (Test-Path $dir)) { New-Item -ItemType Directory -Path $dir | Out-Null }
  if (Test-Path $p) { Copy-Item $p "$p.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')" -Force }
  Set-Content -Path $p -Value $safeBlock -Encoding UTF8
}

# Apply to current session (Windows PowerShell profile path)
. $PROFILE

# One-time npm config (safe re-run)
$npmCmd = Join-Path $NodeHome 'npm.cmd'
if (Test-Path $npmCmd) {
  & $npmCmd config set prefix $NpmPrefix --location=user
  & $npmCmd config set cache  $NpmCache  --location=user
} else {
  $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js'
  & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user
  & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user
}

# Verify
"Node locations (PATH order):"; where.exe node
"node -v:"; node -v
"npm -v:";  npm -v
"npm prefix:"; npm config get prefix
"npm cache:";  npm config get cache

"✅ Repair complete. Open a NEW PowerShell window and try: node -v"
# === end REPAIR ===

