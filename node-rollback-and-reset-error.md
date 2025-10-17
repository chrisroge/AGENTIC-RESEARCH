PS C:\Users\d-christopher.g.roge> # === ROLLBACK portable Node/npm (user-level, no admin) — PS 5.1 SAFE ===
PS C:\Users\d-christopher.g.roge> # Safety toggles (set to $true if you also want to delete folders on disk)
PS C:\Users\d-christopher.g.roge> $RemovePortableNodeDirs = $false   # deletes $NodeHome if true
PS C:\Users\d-christopher.g.roge> $RemoveNpmDirs          = $false   # deletes $NpmPrefix and $NpmCache if true
PS C:\Users\d-christopher.g.roge> $RemoveNVS              = $false   # deletes %LOCALAPPDATA%\nvs if true
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Your paths (note: using lower-case 'tools')
PS C:\Users\d-christopher.g.roge> $NodeHome    = Join-Path $HOME 'tools\node-v22.20.0-win-x64'
PS C:\Users\d-christopher.g.roge> $NpmPrefix   = Join-Path $HOME 'tools\npm-global-22'
PS C:\Users\d-christopher.g.roge> $NpmCache    = Join-Path $HOME 'tools\npm-cache-22'
PS C:\Users\d-christopher.g.roge> $NvsHome     = Join-Path $env:LOCALAPPDATA 'nvs'
PS C:\Users\d-christopher.g.roge> $WindowsApps = Join-Path $env:LOCALAPPDATA 'Microsoft\WindowsApps'
PS C:\Users\d-christopher.g.roge> $AppDataNpm  = Join-Path $env:APPDATA     'npm'
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Backing up and cleaning PowerShell profiles..."
==> Backing up and cleaning PowerShell profiles...
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> function Remove-PortableBlockFromProfile {
>>   param([string]$ProfilePath)
>>   if (-not $ProfilePath) { return }
>>   $dir = Split-Path -Parent $ProfilePath
>>   if (-not (Test-Path $dir)) { return } # profile never created
>>
>>   if (Test-Path $ProfilePath) {
>>     $backup = "$ProfilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
>>     Copy-Item $ProfilePath $backup -Force
>>
>>     $raw = Get-Content $ProfilePath -Raw -ErrorAction SilentlyContinue
>>     if ($raw -match '# >>> portable-node') {
>>       $clean = [regex]::Replace($raw,'(?s)# >>> portable-node.*?# <<< portable-node\s*','')
>>       Set-Content -Path $ProfilePath -Value $clean -Encoding UTF8
>>       Write-Host "   - Removed portable-node block from $ProfilePath"
>>     } else {
>>       Write-Host "   - No portable-node block found in $ProfilePath (skipped)"
>>     }
>>   }
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Targets: Windows PowerShell + PowerShell 7 profile (if it exists)
PS C:\Users\d-christopher.g.roge> $Profiles = @(
>>   $PROFILE,
>>   (Join-Path $HOME 'Documents\PowerShell\Microsoft.PowerShell_profile.ps1')
>> ) | Select-Object -Unique
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $Profiles | ForEach-Object { Remove-PortableBlockFromProfile $_ }
   - Removed portable-node block from C:\Users\d-christopher.g.roge\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
   - No portable-node block found in C:\Users\d-christopher.g.roge\Documents\PowerShell\Microsoft.PowerShell_profile.ps1 (skipped)
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Restoring User PATH..."
==> Restoring User PATH...
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> function Remove-FromPath {
>>   param([string]$PathValue, [string[]]$RemoveStartsWith, [string]$EnsureContains)
>>
>>   if ($null -eq $PathValue) { $PathValue = '' }
>>   $parts = $PathValue -split ';' | Where-Object { $_ -ne '' } | ForEach-Object { $_.Trim() }
>>
>>   # Normalize for comparison
>>   $norm = { param($p) ($p -replace '/', '\').TrimEnd('\') }
>>
>>   $patterns = @()
>>   foreach ($r in $RemoveStartsWith) {
>>     if ($r) { $patterns += (& $norm $r).ToLowerInvariant() }
>>   }
>>
>>   $filtered = @()
>>   foreach ($p in $parts) {
>>     $np = (& $norm $p).ToLowerInvariant()
>>     $startsWithPattern = $false
>>     foreach ($pat in $patterns) {
>>       if ($pat -ne '' -and $np.StartsWith($pat)) { $startsWithPattern = $true; break }
>>     }
>>     if (-not $startsWithPattern) { $filtered += $p }
>>   }
>>
>>   # Ensure WindowsApps is present (typical clean-user PATH)
>>   if ($EnsureContains -and -not ($filtered | Where-Object { $_.Trim().ToLowerInvariant() -eq ((& $norm $EnsureContains).ToLowerInvariant()) })) {
>>     $filtered = @($EnsureContains) + $filtered
>>   }
>>
>>   # Deduplicate while preserving order
>>   $seen = @{}
>>   $deduped = @()
>>   foreach ($p in $filtered) {
>>     $key = ((& $norm $p).ToLowerInvariant())
>>     if (-not $seen.ContainsKey($key)) { $seen[$key] = $true; $deduped += $p }
>>   }
>>   ($deduped -join ';')
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Build removal list (covers both 'tools' and accidental 'Tools', plus NVS and npm shims)
PS C:\Users\d-christopher.g.roge> $removeList = @(
>>   $NodeHome,
>>   $NpmPrefix,
>>   $NpmCache,
>>   (Join-Path $HOME 'tools\node-v'),   # any other portable node-v* under tools
>>   (Join-Path $HOME 'Tools\node-v'),   # cover accidental 'Tools'
>>   $NvsHome,
>>   $AppDataNpm
>> )
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Compute a new *User* PATH (if current is empty, we'll rebuild from scratch)
PS C:\Users\d-christopher.g.roge> $userPath = [Environment]::GetEnvironmentVariable('Path','User')
PS C:\Users\d-christopher.g.roge> if ($null -eq $userPath -or $userPath.Trim() -eq '') {
>>   $newUserPath = $WindowsApps
>> } else {
>>   $newUserPath = Remove-FromPath -PathValue $userPath -RemoveStartsWith $removeList -EnsureContains $WindowsApps
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> [Environment]::SetEnvironmentVariable('Path', $newUserPath, 'User')
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Update this session PATH (non-persistent)
PS C:\Users\d-christopher.g.roge> $machinePath = [Environment]::GetEnvironmentVariable('Path','Machine')
PS C:\Users\d-christopher.g.roge> if ($null -eq $machinePath) { $machinePath = '' }
PS C:\Users\d-christopher.g.roge> $env:Path = ($newUserPath, $machinePath -join ';')
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "   - User PATH updated."
   - User PATH updated.
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Clearing npm user config (prefix/cache) ..."
==> Clearing npm user config (prefix/cache) ...
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Try npm if present; else edit ~/.npmrc
PS C:\Users\d-christopher.g.roge> $gc = Get-Command npm -ErrorAction SilentlyContinue
PS C:\Users\d-christopher.g.roge> $npmExe = $null
PS C:\Users\d-christopher.g.roge> if ($gc) { $npmExe = $gc.Source }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> if ($npmExe) {
>>   try { npm config delete prefix --location=user | Out-Null } catch { }
>>   try { npm config delete cache  --location=user | Out-Null } catch { }
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Also clean ~/.npmrc lines explicitly
PS C:\Users\d-christopher.g.roge> $npmrc = Join-Path $HOME '.npmrc'
PS C:\Users\d-christopher.g.roge> if (Test-Path $npmrc) {
>>   $backupNpmrc = "$npmrc.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
>>   Copy-Item $npmrc $backupNpmrc -Force
>>
>>   $lines = Get-Content $npmrc -ErrorAction SilentlyContinue
>>   $kept  = $lines | Where-Object { $_ -notmatch '^\s*(prefix|cache)\s*=' }
>>   if ($kept.Count -eq 0) {
>>     Remove-Item $npmrc -Force
>>     Write-Host "   - Removed empty $npmrc"
>>   } else {
>>     Set-Content -Path $npmrc -Value $kept -Encoding UTF8
>>     Write-Host "   - Cleaned prefix/cache entries in $npmrc"
>>   }
>> }
   - Removed empty C:\Users\d-christopher.g.roge\.npmrc
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Optional removals (set switches at top to `$true to enable)..."
==> Optional removals (set switches at top to $true to enable)...
PS C:\Users\d-christopher.g.roge> if ($RemovePortableNodeDirs -and (Test-Path $NodeHome)) {
>>   Remove-Item -Recurse -Force $NodeHome
>>   Write-Host "   - Deleted $NodeHome"
>> }
PS C:\Users\d-christopher.g.roge> if ($RemoveNpmDirs) {
>>   foreach ($d in @($NpmPrefix, $NpmCache)) {
>>     if ($d -and (Test-Path $d)) { Remove-Item -Recurse -Force $d; Write-Host "   - Deleted $d" }
>>   }
>> }
PS C:\Users\d-christopher.g.roge> if ($RemoveNVS -and (Test-Path $NvsHome)) {
>>   Remove-Item -Recurse -Force $NvsHome
>>   Write-Host "   - Deleted $NvsHome"
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Verification:"
==> Verification:
PS C:\Users\d-christopher.g.roge> Write-Host "   User PATH now:"
   User PATH now:
PS C:\Users\d-christopher.g.roge> [Environment]::GetEnvironmentVariable('Path','User')
C:\Users\d-christopher.g.roge\AppData\Local\Microsoft\WindowsApps
PS C:\Users\d-christopher.g.roge> Write-Host "   where.exe node:"
   where.exe node:
PS C:\Users\d-christopher.g.roge> where.exe node 2>$null | ForEach-Object { "     $_" }
PS C:\Users\d-christopher.g.roge> Write-Host "   node -v (may be blank if none installed):"
   node -v (may be blank if none installed):
PS C:\Users\d-christopher.g.roge> try { node -v } catch { "     (node not on PATH in this session)" }
     (node not on PATH in this session)
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "`n✅ Rollback complete. Open a NEW Windows PowerShell window."

✅ Rollback complete. Open a NEW Windows PowerShell window.
PS C:\Users\d-christopher.g.roge> # === end ROLLBACK ===
PS C:\Users\d-christopher.g.roge> # === Portable Node + npm (no-admin) — CLEAN SETUP ===
PS C:\Users\d-christopher.g.roge> # 0) Adjust these if you use a different Node folder/version:
PS C:\Users\d-christopher.g.roge> $NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
PS C:\Users\d-christopher.g.roge> $NpmPrefix = "$HOME\tools\npm-global-22"
PS C:\Users\d-christopher.g.roge> $NpmCache  = "$HOME\tools\npm-cache-22"
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 1) Validate portable Node exists
PS C:\Users\d-christopher.g.roge> if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) {
>>   Write-Error "node.exe not found at $NodeHome. Update `$NodeHome or unzip the Node win-x64 ZIP there."
>>   return
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 2) Ensure npm global + cache dirs exist (one-time)
PS C:\Users\d-christopher.g.roge> New-Item -ItemType Directory -Force -Path $NpmPrefix, $NpmCache | Out-Null
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 3) Write a SAFE profile block (no expansion at write time)
PS C:\Users\d-christopher.g.roge> $profilePath = $PROFILE   # e.g. ...\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

PS C:\Users\d-christopher.g.roge> $backup = "$profilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
PS C:\Users\d-christopher.g.roge> if (Test-Path $profilePath) { Copy-Item $profilePath $backup -Force }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $profileBlock = @'
>> # >>> portable-node (no-admin)
>> # NOTE: This block is written with a *single-quoted* here-string so variables expand only at shell startup.
>> $nodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
>> $npmPrefix = "$HOME\tools\npm-global-22"
>> $npmCache  = "$HOME\tools\npm-cache-22"
>>
>> if (Test-Path (Join-Path $nodeHome 'node.exe')) {
>>   # Put portable Node first on PATH
>>   $env:Path = "$nodeHome;$env:Path"
>>   # Remove common hijackers (system Node and WindowsApps shims)
>>   $env:Path = ($env:Path -split ';' | Where-Object { $_ -and $_ -notmatch '\\Program Files\\nodejs|\\WindowsApps' }) -join ';'
>> }
>>
>> # Prefer user npm "global" bin dir if present (Windows puts shims directly in prefix)
>> if (Test-Path $npmPrefix) {
>>   $env:Path = "$npmPrefix;$env:Path"
>> }
>> # <<< portable-node
>> '@
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Make sure profile folder exists, then write
PS C:\Users\d-christopher.g.roge> $profileDir = Split-Path -Parent $profilePath
PS C:\Users\d-christopher.g.roge> if (-not (Test-Path $profileDir)) { New-Item -ItemType Directory -Path $profileDir | Out-Null }
PS C:\Users\d-christopher.g.roge> Set-Content -Path $profilePath -Value $profileBlock -Encoding UTF8
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 4) Apply the profile to *this* session
PS C:\Users\d-christopher.g.roge> . $profilePath
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 5) One-time npm config to keep everything in your user folders (no admin)
PS C:\Users\d-christopher.g.roge> # Invoke npm from the portable Node directly so we don't depend on PATH yet
PS C:\Users\d-christopher.g.roge> $npmCmd = Join-Path $NodeHome 'npm.cmd'
PS C:\Users\d-christopher.g.roge> if (Test-Path $npmCmd) {
>>   & $npmCmd config set prefix $NpmPrefix --location=user
>>   & $npmCmd config set cache  $NpmCache  --location=user
>> } else {
>>   # Fallback: call npm via node.exe + npm-cli.js
>>   $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js'
>>   & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user
>>   & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 6) Verify
PS C:\Users\d-christopher.g.roge> "Node locations (PATH order):"; where.exe node
Node locations (PATH order):
C:\Users\d-christopher.g.roge\tools\node-v22.20.0-win-x64\node.exe
PS C:\Users\d-christopher.g.roge> "node -v:"; node -v
node -v:
v22.20.0
PS C:\Users\d-christopher.g.roge> "npm -v:";  npm -v
npm -v:
10.9.3
PS C:\Users\d-christopher.g.roge> "npm prefix:"; npm config get prefix
npm prefix:
C:\Users\d-christopher.g.roge\tools\npm-global-22
PS C:\Users\d-christopher.g.roge> "npm cache:";  npm config get cache
npm cache:
C:\Users\d-christopher.g.roge\tools\npm-cache-22
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> "✅ Setup complete. Open a NEW PowerShell window; node/npm should be ready in every session."
✅ Setup complete. Open a NEW PowerShell window; node/npm should be ready in every session.
PS C:\Users\d-christopher.g.roge> # === end CLEAN SETUP ===
PS C:\Users\d-christopher.g.roge> claude
claude : The term 'claude' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the
spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ claude
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (claude:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\d-christopher.g.roge> node -v
v22.20.0
PS C:\Users\d-christopher.g.roge>
