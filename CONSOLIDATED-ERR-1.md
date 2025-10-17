Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Users\d-christopher.g.roge> claude
claude : The term 'claude' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ claude
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (claude:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\d-christopher.g.roge> # === INSTALL: Portable Node + npm + Claude CLI + VS Code compatibility (PS 5.1 safe) ===
PS C:\Users\d-christopher.g.roge> # Adjust if you use different paths/versions
PS C:\Users\d-christopher.g.roge> $NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
PS C:\Users\d-christopher.g.roge> $NpmPrefix = "$HOME\tools\npm-global-22"
PS C:\Users\d-christopher.g.roge> $NpmCache  = "$HOME\tools\npm-cache-22"
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 0) Validate Node exists
PS C:\Users\d-christopher.g.roge> if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) { Write-Error "node.exe not found at $NodeHome"; return }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 1) Ensure npm dirs
PS C:\Users\d-christopher.g.roge> New-Item -ItemType Directory -Force -Path $NpmPrefix,$NpmCache | Out-Null
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 2) Put portable Node/npm-global/Git\bin on USER PATH (and optionally Machine PATH for Git\bin)
PS C:\Users\d-christopher.g.roge> $AddGitToSystemPath = $false   # set $true if you can run as Administrator
PS C:\Users\d-christopher.g.roge> $gitBinShort = 'C:\Progra~1\Git\bin'
PS C:\Users\d-christopher.g.roge> $gitBinLong  = Join-Path $env:ProgramFiles 'Git\bin'
PS C:\Users\d-christopher.g.roge> $gitBinX86   = Join-Path ${env:ProgramFiles(x86)} 'Git\bin'
PS C:\Users\d-christopher.g.roge> if     (Test-Path $gitBinShort) { $GitBin = $gitBinShort }
PS C:\Users\d-christopher.g.roge> elseif (Test-Path $gitBinLong)  { $GitBin = $gitBinLong  }
elseif : The term 'elseif' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ elseif (Test-Path $gitBinLong)  { $GitBin = $gitBinLong  }
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (elseif:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\d-christopher.g.roge> elseif (Test-Path $gitBinX86)   { $GitBin = $gitBinX86   }
elseif : The term 'elseif' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ elseif (Test-Path $gitBinX86)   { $GitBin = $gitBinX86   }
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (elseif:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\d-christopher.g.roge> else   { $GitBin = $null }
else : The term 'else' is not recognized as the name of a cmdlet, function, script file, or operable program. Check
the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ else   { $GitBin = $null }
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (else:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> function Prepend-Entries {
>>   param([string]$current, [string[]]$entries)
>>   if ($null -eq $current) { $current = '' }
>>   $parts = ($current -split ';') | Where-Object { $_ } | ForEach-Object { $_.Trim() }
>>   $norm  = { param($p) ($p -replace '/', '\').TrimEnd('\').ToLowerInvariant() }
>>   $targets = @(); foreach ($e in $entries) { if ($e) { $targets += (& $norm $e) } }
>>   $parts = $parts | Where-Object { $np = (& $norm $_); -not ($targets | Where-Object { $_ -eq $np }) }
>>   ($entries + $parts) -join ';'
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $currUser = [Environment]::GetEnvironmentVariable('Path','User')
PS C:\Users\d-christopher.g.roge> $entries  = @($NodeHome, $NpmPrefix)
PS C:\Users\d-christopher.g.roge> if ($GitBin) { $entries += $GitBin }
PS C:\Users\d-christopher.g.roge> $newUser  = Prepend-Entries -current $currUser -entries $entries
PS C:\Users\d-christopher.g.roge> [Environment]::SetEnvironmentVariable('Path', $newUser, 'User')
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> if ($AddGitToSystemPath -and $GitBin) {
>>   try {
>>     $currMach = [Environment]::GetEnvironmentVariable('Path','Machine')
>>     $newMach  = Prepend-Entries -current $currMach -entries @($GitBin)
>>     [Environment]::SetEnvironmentVariable('Path', $newMach, 'Machine')
>>     Write-Host "Machine PATH updated with $GitBin"
>>   } catch { Write-Warning "Admin required to modify Machine PATH. Skipped." }
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 3) Make VS Code & new apps see env changes
PS C:\Users\d-christopher.g.roge> Add-Type -Namespace Win32 -Name Native -MemberDefinition @"
>> [System.Runtime.InteropServices.DllImport("user32.dll", SetLastError=true, CharSet=System.Runtime.InteropServices.CharSet.Auto)]
>> public static extern System.IntPtr SendMessageTimeout(System.IntPtr hWnd, uint Msg, System.UIntPtr wParam, string lParam, uint fuFlags, uint uTimeout, out System.UIntPtr lpdwResult);
>> "@
PS C:\Users\d-christopher.g.roge> $null = [Win32.Native]::SendMessageTimeout([IntPtr]0xffff,0x1A,[UIntPtr]::Zero,"Environment",0x0002,5000,[ref]([UIntPtr]::Zero))
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 4) Write a SAFE PowerShell profile block (single-quoted here-string)
PS C:\Users\d-christopher.g.roge> $profilePath = $PROFILE
PS C:\Users\d-christopher.g.roge> $profileDir  = Split-Path -Parent $profilePath
PS C:\Users\d-christopher.g.roge> if (-not (Test-Path $profileDir)) { New-Item -ItemType Directory -Path $profileDir | Out-Null }
PS C:\Users\d-christopher.g.roge> if (Test-Path $profilePath) { Copy-Item $profilePath "$profilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')" -Force }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> $profileBlock = @'
>> # >>> portable-node (no-admin)
>> $nodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
>> $npmPrefix = "$HOME\tools\npm-global-22"
>>
>> if (Test-Path (Join-Path $nodeHome 'node.exe')) {
>>   $env:Path = "$nodeHome;$env:Path"
>> }
>> if (Test-Path $npmPrefix) {
>>   $env:Path = "$npmPrefix;$env:Path"
>> }
>> # <<< portable-node
>> '@
PS C:\Users\d-christopher.g.roge> Set-Content -Path $profilePath -Value $profileBlock -Encoding UTF8
PS C:\Users\d-christopher.g.roge> . $profilePath  # apply to this session
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 5) npm config (user-level, portable)
PS C:\Users\d-christopher.g.roge> $npmCmd = Join-Path $NodeHome 'npm.cmd'
PS C:\Users\d-christopher.g.roge> if (Test-Path $npmCmd) {
>>   & $npmCmd config set prefix $NpmPrefix --location=user
>>   & $npmCmd config set cache  $NpmCache  --location=user
>> } else {
>>   $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js'
>>   & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user
>>   & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user
>> }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 6) Ensure current shell sees npm-global, then install Claude CLI
PS C:\Users\d-christopher.g.roge> $env:Path = "$NpmPrefix;$env:Path"
PS C:\Users\d-christopher.g.roge> npm i -g @anthropic-ai/claude-code | Out-Null
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # (Optional) Hint Claude Code where Git Bash is
PS C:\Users\d-christopher.g.roge> if ($GitBin) { [Environment]::SetEnvironmentVariable('CLAUDE_CODE_GIT_BASH_PATH', (Join-Path $GitBin 'bash.exe'), 'User') }
PS C:\Users\d-christopher.g.roge>
PS C:\Users\d-christopher.g.roge> # 7) Verify
PS C:\Users\d-christopher.g.roge> "node -v:"; node -v
node -v:
v22.20.0
PS C:\Users\d-christopher.g.roge> "npm -v:";  npm -v
npm -v:
10.9.3
PS C:\Users\d-christopher.g.roge> "where node:"; where.exe node
where node:
C:\Users\d-christopher.g.roge\tools\node-v22.20.0-win-x64\node.exe
PS C:\Users\d-christopher.g.roge> "where claude:"; where.exe claude
where claude:
C:\Users\d-christopher.g.roge\tools\npm-global-22\claude
C:\Users\d-christopher.g.roge\tools\npm-global-22\claude.cmd
PS C:\Users\d-christopher.g.roge> "Done. If using VS Code: File → Exit, reopen, then test the Claude Code extension."
Done. If using VS Code: File → Exit, reopen, then test the Claude Code extension.
PS C:\Users\d-christopher.g.roge> # === end INSTALL ===
PS C:\Users\d-christopher.g.roge>
