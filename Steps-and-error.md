Downgrade Node to Long Term Support (LTS) version 22.20.0 

Due to forward compatibility issues with the latest version of Node provided through Software Center some Claude Code users may experience errors when launching Claude Code. As a temporary work-around you may download and use the LTS version (22.20.0) with the following instructions: 

Download the LTS standalone zip from https://nodejs.org/dist/v22.20.0/node-v22.20.0-win-x64.zip  

Create ‘tools’ folder within C:\Users\d-[YOUR.USER.NAME] 

Unzip to C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64 
 
*Note: Once extracted, be sure that the file path reads as follows: 
C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64 
 
If your path reads: 
C:\Users\d-[YOUR.USER.NAME]\tools\node-v22.20.0-win-x64\node-v22.20.0-win-x64 
all files are nested in an additional folder. Simply cut the nests folder and paste it into the ‘tools’ folder to remove the duplicate folder. 
 

Uninstall Node from Software Center 

Copy and paste the following commands in a Windows PowerShell terminal: 
 

# === INSTALL: Portable Node 22.20.0 + npm + Claude CLI (PS 5.1 safe) === 

# Adjust these if your paths/versions differ 

$NodeHome  = "$HOME\tools\node-v22.20.0-win-x64" 

$NpmPrefix = "$HOME\tools\npm-global-22" 

$NpmCache  = "$HOME\tools\npm-cache-22" 

 

# Validate portable Node exists 

if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) { Write-Error "node.exe not found at $NodeHome"; return } 

 

# Ensure npm global/cache dirs 

New-Item -ItemType Directory -Force -Path $NpmPrefix,$NpmCache | Out-Null 

 

# Resolve Git\bin (short or long path) and (optionally) add to PATH 

$GitBin = @( 

  'C:\Progra~1\Git\bin', 

  "$env:ProgramFiles\Git\bin", 

  "${env:ProgramFiles(x86)}\Git\bin", 

  "$env:LOCALAPPDATA\Programs\Git\bin" 

) | Where-Object { Test-Path $_ } | Select-Object -First 1 

 

# Toggle: also add Git\bin to SYSTEM PATH (requires admin). Leave $false for user-only. 

$AddGitToSystemPath = $false 

 

# Helper: prepend entries to a PATH string (dedupe, preserve order) 

function Prepend-Entries { 

  param([string]$current, [string[]]$entries) 

  if ($null -eq $current) { $current = '' } 

  $parts = ($current -split ';') | Where-Object { $_ } | ForEach-Object { $_.Trim() } 

  $norm  = { param($p) ($p -replace '/', '\').TrimEnd('\').ToLowerInvariant() } 

  $targets = @(); foreach ($e in $entries) { if ($e) { $targets += (& $norm $e) } } 

  $parts = $parts | Where-Object { $np = (& $norm $_); -not ($targets | Where-Object { $_ -eq $np }) } 

  ($entries + $parts) -join ';' 

} 

 

# Put Node/npm-global/(Git\bin if found) on USER PATH 

$currUser = [Environment]::GetEnvironmentVariable('Path','User') 

$entries  = @($NodeHome, $NpmPrefix) 

if ($GitBin) { $entries += $GitBin } 

$newUser  = Prepend-Entries -current $currUser -entries $entries 

[Environment]::SetEnvironmentVariable('Path', $newUser, 'User') 

 

# Optionally add Git\bin to SYSTEM PATH (needs admin) 

if ($AddGitToSystemPath -and $GitBin) { 

  try { 

    $currMach = [Environment]::GetEnvironmentVariable('Path','Machine') 

    $newMach  = Prepend-Entries -current $currMach -entries @($GitBin) 

    [Environment]::SetEnvironmentVariable('Path', $newMach, 'Machine') 

    Write-Host "Machine PATH updated with $GitBin" 

  } catch { Write-Warning "Admin required to modify Machine PATH. Skipped." } 

} 

 

# Broadcast env change so GUI apps (VS Code) see it 

Add-Type -Namespace Win32 -Name Native -MemberDefinition @" 

[System.Runtime.InteropServices.DllImport("user32.dll", SetLastError=true, CharSet=System.Runtime.InteropServices.CharSet.Auto)] 

public static extern System.IntPtr SendMessageTimeout(System.IntPtr hWnd, uint Msg, System.UIntPtr wParam, string lParam, uint fuFlags, uint uTimeout, out System.UIntPtr lpdwResult); 

"@ 

$null = [Win32.Native]::SendMessageTimeout([IntPtr]0xffff,0x1A,[UIntPtr]::Zero,"Environment",0x0002,5000,[ref]([UIntPtr]::Zero)) 

 

# 5) Safe PowerShell profile: prepend Node/npm-global for each new shell 

$profilePath = $PROFILE 

$profileDir  = Split-Path -Parent $profilePath 

if (-not (Test-Path $profileDir)) { New-Item -ItemType Directory -Path $profileDir | Out-Null } 

if (Test-Path $profilePath) { Copy-Item $profilePath "$profilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')" -Force } 

 

$profileBlock = @' 

# >>> portable-node (no-admin) 

$nodeHome  = "$HOME\tools\node-v22.20.0-win-x64" 

$npmPrefix = "$HOME\tools\npm-global-22" 

if (Test-Path (Join-Path $nodeHome 'node.exe')) { $env:Path = "$nodeHome;$env:Path" } 

if (Test-Path $npmPrefix) { $env:Path = "$npmPrefix;$env:Path" } 

# <<< portable-node 

'@ 

Set-Content -Path $profilePath -Value $profileBlock -Encoding UTF8 

. $profilePath  # apply now 

 

# npm config (user-level) 

$npmCmd = Join-Path $NodeHome 'npm.cmd' 

if (Test-Path $npmCmd) { 

  & $npmCmd config set prefix $NpmPrefix --location=user 

  & $npmCmd config set cache  $NpmCache  --location=user 

} else { 

  $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js' 

  & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user 

  & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user 

} 

 

# Ensure this session sees npm-global and install Claude CLI 

$env:Path = "$NpmPrefix;$env:Path" 

npm i -g @anthropic-ai/claude-code | Out-Null 

 

# Hint Claude Code where Git Bash is (unquoted) for the VS Code extension 

if ($GitBin) { [Environment]::SetEnvironmentVariable('CLAUDE_CODE_GIT_BASH_PATH', (Join-Path $GitBin 'bash.exe'), 'User') } 

 

# Verify 

"node -v:"; node -v 

"npm -v:";  npm -v 

"where node:"; where.exe node 

"where claude:"; where.exe claude 

"Done. If using VS Code: File → Exit, reopen, then test the Claude Code extension." 

# === end INSTALL === 

 
 
 
 

Rollback script – Run to reverse these changes and no longer point to the portable Node solution 

# === ROLLBACK: remove portable-node setup (user-level; PS 5.1 safe) === 

$NodeHome    = Join-Path $HOME 'tools\node-v22.20.0-win-x64' 

$NpmPrefix   = Join-Path $HOME 'tools\npm-global-22' 

$NpmCache    = Join-Path $HOME 'tools\npm-cache-22' 

$WindowsApps = Join-Path $env:LOCALAPPDATA 'Microsoft\WindowsApps' 

$AppDataNpm  = Join-Path $env:APPDATA 'npm'   # legacy global shims (leave if you still use it) 

 

# Remove the portable-node block from PowerShell profiles 

function Remove-PortableBlock { 

  param([string]$ProfilePath) 

  $dir = Split-Path -Parent $ProfilePath 

  if (-not (Test-Path $dir) -or -not (Test-Path $ProfilePath)) { return } 

  Copy-Item $ProfilePath "$ProfilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')" -Force 

  $raw = Get-Content $ProfilePath -Raw -ErrorAction SilentlyContinue 

  $clean = [regex]::Replace($raw,'(?s)# >>> portable-node.*?# <<< portable-node\s*','') 

  Set-Content -Path $ProfilePath -Value $clean -Encoding UTF8 

} 

Remove-PortableBlock $PROFILE 

Remove-PortableBlock (Join-Path $HOME 'Documents\PowerShell\Microsoft.PowerShell_profile.ps1')  # pwsh profile, if present 

 

# Remove portable paths from USER PATH; keep WindowsApps so modern apps work 

function Remove-FromPath { 

  param([string]$PathValue,[string[]]$RemoveStartsWith,[string]$EnsureContains) 

  if ($null -eq $PathValue) { $PathValue = '' } 

  $parts = $PathValue -split ';' | Where-Object { $_ } | ForEach-Object { $_.Trim() } 

  $norm  = { param($p) ($p -replace '/', '\').TrimEnd('\') } 

  $patterns = @(); foreach ($r in $RemoveStartsWith) { if ($r) { $patterns += (& $norm $r).ToLowerInvariant() } } 

  $filtered = @() 

  foreach ($p in $parts) { 

    $np = (& $norm $p).ToLowerInvariant() 

    $match = $false; foreach ($pat in $patterns) { if ($pat -ne '' -and $np.StartsWith($pat)) { $match=$true; break } } 

    if (-not $match) { $filtered += $p } 

  } 

  if ($EnsureContains -and -not ($filtered | Where-Object { $_.Trim().ToLowerInvariant() -eq ((& $norm $EnsureContains).ToLowerInvariant()) })) { 

    $filtered = @($EnsureContains) + $filtered 

  } 

  # dedupe, preserve order 

  $seen=@{}; $deduped=@(); foreach($p in $filtered){ $k=((& $norm $p).ToLowerInvariant()); if(-not $seen.ContainsKey($k)){ $seen[$k]=$true; $deduped+=$p } } 

  ($deduped -join ';') 

} 

$removeList = @( 

  $NodeHome, $NpmPrefix, $NpmCache, 

  (Join-Path $HOME 'tools\node-v'), 

  (Join-Path $HOME 'Tools\node-v') 

) 

$userPath = [Environment]::GetEnvironmentVariable('Path','User') 

$newUser  = if ($null -eq $userPath -or $userPath.Trim() -eq '') { $WindowsApps } else { Remove-FromPath -PathValue $userPath -RemoveStartsWith $removeList -EnsureContains $WindowsApps } 

[Environment]::SetEnvironmentVariable('Path', $newUser, 'User') 

 

# Clear npm user config for prefix/cache and tidy ~/.npmrc 

$gc = Get-Command npm -ErrorAction SilentlyContinue 

if ($gc) { try { npm config delete prefix --location=user | Out-Null } catch {}; try { npm config delete cache --location=user | Out-Null } catch {} } 

$npmrc = Join-Path $HOME '.npmrc' 

if (Test-Path $npmrc) { 

  $lines = Get-Content $npmrc -ErrorAction SilentlyContinue | Where-Object { $_ -notmatch '^\s*(prefix|cache)\s*=' } 

  if ($lines.Count -eq 0) { Remove-Item $npmrc -Force } else { Set-Content -Path $npmrc -Value $lines -Encoding UTF8 } 

} 

 

# Broadcast env change and verify 

Add-Type -Namespace Win32 -Name Native -MemberDefinition @" 

[System.Runtime.InteropServices.DllImport("user32.dll", SetLastError=true, CharSet=System.Runtime.InteropServices.CharSet.Auto)] 

public static extern System.IntPtr SendMessageTimeout(System.IntPtr hWnd, uint Msg, System.UIntPtr wParam, string lParam, uint fuFlags, uint uTimeout, out System.UIntPtr lpdwResult); 

"@ 

$null = [Win32.Native]::SendMessageTimeout([IntPtr]0xffff,0x1A,[UIntPtr]::Zero,"Environment",0x0002,5000,[ref]([UIntPtr]::Zero)) 

 

"User PATH now:"; [Environment]::GetEnvironmentVariable('Path','User') 

"where node:";    where.exe node 2>$null | ForEach-Object { "  $_" } 

"node -v (may be empty if no system Node):" 

try { node -v } catch { "(node not on PATH)" } 

"✅ Rollback complete. Open a NEW Windows PowerShell window." 

# === end ROLLBACK === 




I followed the steps and still get this when curl-ing claudeai
 
curl -v https://claude.ai
* Host claude.ai:443 was resolved.
* IPv6: (none)
* IPv4: 160.79.104.10
*   Trying 160.79.104.10:443...
* Connected to claude.ai (160.79.104.10) port 443
* ALPN: curl offers h2,http/1.1
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
*  CAfile: /etc/ssl/certs/ca-certificates.crt
*  CApath: /etc/ssl/certs
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (OUT), TLS alert, unknown CA (560):
* SSL certificate problem: unable to get local issuer certificate
* Closing connection
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.se/docs/sslcerts.html
 
curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.

 
