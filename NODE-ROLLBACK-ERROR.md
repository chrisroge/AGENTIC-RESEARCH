PS C:\Users\d-christopher.g.roge> # === Roll back portable Node/npm setup (user-level, no admin) ===
PS C:\Users\d-christopher.g.roge> # SAFETY SWITCHES (set to $true to also delete the on-disk folders)
PS C:\Users\d-christopher.g.roge> $RemovePortableNodeDirs = $false   # deletes $NodeHome if true
PS C:\Users\d-christopher.g.roge> $RemoveNpmDirs          = $false   # deletes $NpmPrefix and $NpmCache if true
PS C:\Users\d-christopher.g.roge> $RemoveNVS              = $false   # deletes %LOCALAPPDATA%\nvs if true
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Paths you likely used (edit if yours differ)
PS C:\Users\d-christopher.g.roge> $NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
PS C:\Users\d-christopher.g.roge> $NpmPrefix = "$HOME\tools\npm-global-22"
PS C:\Users\d-christopher.g.roge> $NpmCache  = "$HOME\tools\npm-cache-22"
PS C:\Users\d-christopher.g.roge> $NvsHome   = Join-Path $env:LOCALAPPDATA 'nvs'
PS C:\Users\d-christopher.g.roge> $WindowsApps = Join-Path $env:LOCALAPPDATA 'Microsoft\WindowsApps'
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
>>       $clean = [regex]::Replace(
>>         $raw,
>>         '(?s)# >>> portable-node.*?# <<< portable-node\s*',
>>         ''
>>       )
>>       Set-Content -Path $ProfilePath -Value $clean -Encoding UTF8
>>       Write-Host "   - Removed portable-node block from $ProfilePath"
>>     } else {
>>       Write-Host "   - No portable-node block found in $ProfilePath (skipped)"
>>     }
>>   }
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Targets: Windows PowerShell + PowerShell 7 profile paths
PS C:\Users\d-christopher.g.roge> $Profiles = @()
PS C:\Users\d-christopher.g.roge> $Profiles += $PROFILE
PS C:\Users\d-christopher.g.roge> $Profiles += (Join-Path $HOME 'Documents\PowerShell\Microsoft.PowerShell_profile.ps1') # pwsh
PS C:\Users\d-christopher.g.roge> $Profiles = $Profiles | Select-Object -Unique
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $Profiles | ForEach-Object { Remove-PortableBlockFromProfile $_ }
   - No portable-node block found in C:\Users\d-christopher.g.roge\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 (skipped)
   - No portable-node block found in C:\Users\d-christopher.g.roge\Documents\PowerShell\Microsoft.PowerShell_profile.ps1 (skipped)
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Restoring User PATH..."
==> Restoring User PATH...
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Helper: remove entries that start with any of the given patterns (case-insensitive)
PS C:\Users\d-christopher.g.roge> function Remove-FromPath {
>>   param([string]$PathValue, [string[]]$RemoveStartsWith, [string]$EnsureContains)
>>   $parts = $PathValue -split ';' | Where-Object { $_ -ne '' } | ForEach-Object { $_.Trim() }
>>
>>   # Normalize slashes for comparison
>>   $norm = { param($p) ($p -replace '/', '\').TrimEnd('\') }
>>
>>   $patterns = @()
>>   foreach ($r in $RemoveStartsWith) {
>>     if ($r) { $patterns += (& $norm $r).ToLowerInvariant() }
>>   }
>>
>>   $filtered = foreach ($p in $parts) {
>>     $np = (& $norm $p).ToLowerInvariant()
>>     $startsWithPattern = $false
>>     foreach ($pat in $patterns) {
>>       if ($pat -ne '' -and $np.StartsWith($pat)) { $startsWithPattern = $true; break }
>>     }
>>     if (-not $startsWithPattern) { $p }
>>   }
>>
>>   # Ensure WindowsApps is present (common default in a clean user PATH)
>>   if ($EnsureContains -and -not ($filtered | Where-Object { $_.Trim().ToLowerInvariant() -eq ((& $norm $EnsureContains).ToLowerInvariant()) })) {
>>     $filtered = @($EnsureContains) + $filtered
>>   }
>>
>>   # Deduplicate while preserving order
>>   $seen = @{}
>>   $deduped = foreach ($p in $filtered) {
>>     $key = ((& $norm $p).ToLowerInvariant())
>>     if (-not $seen.ContainsKey($key)) { $seen[$key] = $true; $p }
>>   }
>>   ($deduped -join ';')
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Compute a conservative list of things to remove from *User* PATH
PS C:\Users\d-christopher.g.roge> $removeList = @(
>>   $NodeHome,
>>   $NpmPrefix,
>>   (Join-Path $HOME 'T]tools\node-v'),   # any other portable node-v* under tools
>>   $NvsHome
>> )
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $userPath = [Environment]::GetEnvironmentVariable('Path', 'User')
PS C:\Users\d-christopher.g.roge> $newUserPath = Remove-FromPath -PathValue ($userPath ?? '') -RemoveStartsWith $removeList -EnsureContains $WindowsApps
At line:1 char:54
+ $newUserPath = Remove-FromPath -PathValue ($userPath ?? '') -RemoveSt ...
+                                                      ~~
Unexpected token '??' in expression or statement.
At line:1 char:53
+ $newUserPath = Remove-FromPath -PathValue ($userPath ?? '') -RemoveSt ...
+                                                     ~
Missing closing ')' in expression.
At line:1 char:59
+ $newUserPath = Remove-FromPath -PathValue ($userPath ?? '') -RemoveSt ...
+                                                           ~
Unexpected token ')' in expression or statement.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : UnexpectedToken

PS C:\Users\d-christopher.g.roge> [Environment]::SetEnvironmentVariable('Path', $newUserPath, 'User')
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Update current session PATH similarly (non-persistent)
PS C:\Users\d-christopher.g.roge> $processPath = $env:Path
PS C:\Users\d-christopher.g.roge> # Rebuild current PATH as: newUserPath + Machine PATH (approximate)
PS C:\Users\d-christopher.g.roge> $machinePath = [Environment]::GetEnvironmentVariable('Path','Machine')
PS C:\Users\d-christopher.g.roge> $env:Path = ($newUserPath, $machinePath -join ';')
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "   - User PATH updated."
   - User PATH updated.
PS C:\Users\d-christopher.g.roge> Write-Host "==> Clearing npm user config (prefix/cache) ..."
==> Clearing npm user config (prefix/cache) ...
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Prefer calling npm if available; otherwise edit ~/.npmrc directly
PS C:\Users\d-christopher.g.roge> $npmExe = (Get-Command npm -ErrorAction SilentlyContinue)?.Source
At line:1 char:58
+ $npmExe = (Get-Command npm -ErrorAction SilentlyContinue)?.Source
+                                                          ~~~~~~~~
Unexpected token '?.Source' in expression or statement.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : UnexpectedToken

PS C:\Users\d-christopher.g.roge> $npmrc  = Join-Path $HOME '.npmrc'
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> if ($npmExe) {
>>   try {
>>     npm config delete prefix --location=user | Out-Null
>>     npm config delete cache  --location=user | Out-Null
>>   } catch { }
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # Also clean ~/.npmrc lines explicitly
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
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "==> Optional removals (set switches at top to `$true to enable)..."
==> Optional removals (set switches at top to $true to enable)...
PS C:\Users\d-christopher.g.roge>
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
PS C:\Users\d-christopher.g.roge> Write-Host "   where.exe node:"
   where.exe node:
PS C:\Users\d-christopher.g.roge> where.exe node 2>$null | ForEach-Object { "     $_" }
PS C:\Users\d-christopher.g.roge> Write-Host "   Current node -v (may be blank if none installed):"
   Current node -v (may be blank if none installed):
PS C:\Users\d-christopher.g.roge> try { node -v } catch { "     (node not on PATH for this session)" }
     (node not on PATH for this session)
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> Write-Host "`n✅ Rollback complete."

✅ Rollback complete.
PS C:\Users\d-christopher.g.roge> Write-Host "Open a NEW PowerShell window to pick up the restored User PATH."
Open a NEW PowerShell window to pick up the restored User PATH.
PS C:\Users\d-christopher.g.roge> # === end rollback ===
