PS C:\Users\REDACTED.USER> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
PS C:\Users\REDACTED.USER> # === Portable Node + npm (no-admin)  CLEAN SETUP ===
PS C:\Users\REDACTED.USER> # 0) Adjust these if you use a different Node folder/version:
PS C:\Users\REDACTED.USER> $NodeHome  = "$HOME\tools\node-v22.20.0-win-x64"
PS C:\Users\REDACTED.USER> $NpmPrefix = "$HOME\tools\npm-global-22"
PS C:\Users\REDACTED.USER> $NpmCache  = "$HOME\tools\npm-cache-22"
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 1) Validate portable Node exists
PS C:\Users\REDACTED.USER> if (-not (Test-Path (Join-Path $NodeHome 'node.exe'))) {
>>   Write-Error "node.exe not found at $NodeHome. Update `$NodeHome or unzip the Node win-x64 ZIP there."
>>   return
>> }
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 2) Ensure npm global + cache dirs exist (one-time)
PS C:\Users\REDACTED.USER> New-Item -ItemType Directory -Force -Path $NpmPrefix, $NpmCache | Out-Null
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 3) Write a SAFE profile block (no expansion at write time)
PS C:\Users\REDACTED.USER> $profilePath = $PROFILE   # e.g. ...\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
PS C:\Users\REDACTED.USER> $backup = "$profilePath.bak-$(Get-Date -Format 'yyyyMMdd-HHmmss')"
PS C:\Users\REDACTED.USER> if (Test-Path $profilePath) { Copy-Item $profilePath $backup -Force }
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> $profileBlock = @'
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
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # Make sure profile folder exists, then write
PS C:\Users\REDACTED.USER> $profileDir = Split-Path -Parent $profilePath
PS C:\Users\REDACTED.USER> if (-not (Test-Path $profileDir)) { New-Item -ItemType Directory -Path $profileDir | Out-Null }
PS C:\Users\REDACTED.USER> Set-Content -Path $profilePath -Value $profileBlock -Encoding UTF8
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 4) Apply the profile to *this* session
PS C:\Users\REDACTED.USER> . $profilePath
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 5) One-time npm config to keep everything in your user folders (no admin)
PS C:\Users\REDACTED.USER> # Invoke npm from the portable Node directly so we don't depend on PATH yet
PS C:\Users\REDACTED.USER> $npmCmd = Join-Path $NodeHome 'npm.cmd'
PS C:\Users\REDACTED.USER> if (Test-Path $npmCmd) {
>> & $npmCmd config set prefix $NpmPrefix --location=user
>> & $npmCmd config set cache  $NpmCache  --location=user
>> } else {
>>   # Fallback: call npm via node.exe + npm-cli.js
>>   $npmCli = Join-Path $NodeHome 'node_modules\npm\bin\npm-cli.js'
>> & (Join-Path $NodeHome 'node.exe') $npmCli config set prefix $NpmPrefix --location=user
>> & (Join-Path $NodeHome 'node.exe') $npmCli config set cache  $NpmCache  --location=user
>> }
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # 6) Verify
PS C:\Users\REDACTED.USER> "Node locations (PATH order):"; where.exe node
Node locations (PATH order):
C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\node.exe
PS C:\Users\REDACTED.USER> "node -v:"; node -v
node -v:
v22.20.0
PS C:\Users\REDACTED.USER> "npm -v:";  npm -v
npm -v:
npm : File C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 cannot be loaded. The file
C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 is not digitally signed. You cannot run this script on
the current system. For more information about running scripts and setting execution policy, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:13
+ "npm -v:";  npm -v
+             ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\REDACTED.USER> "npm prefix:"; npm config get prefix
npm prefix:
npm : File C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 cannot be loaded. The file
C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 is not digitally signed. You cannot run this script on
the current system. For more information about running scripts and setting execution policy, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:16
+ "npm prefix:"; npm config get prefix
+                ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\REDACTED.USER> "npm cache:";  npm config get cache
npm cache:
npm : File C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 cannot be loaded. The file
C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 is not digitally signed. You cannot run this script on
the current system. For more information about running scripts and setting execution policy, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:16
+ "npm cache:";  npm config get cache
+                ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> " Setup complete. Open a NEW PowerShell window; node/npm should be ready in every session."
Setup complete. Open a NEW PowerShell window; node/npm should be ready in every session.
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # Make sure the new prefix is on PATH in this session (profile already handles future shells)
PS C:\Users\REDACTED.USER> $env:Path = "$HOME\tools\npm-global-22;$env:Path"
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # Install claude into the *current* npm prefix (C:\Users\<you>\tools\npm-global-22)
PS C:\Users\REDACTED.USER> npm i -g @anthropic-ai/claude-code
npm : File C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 cannot be loaded. The file
C:\Users\REDACTED.USER\tools\node-v22.20.0-win-x64\npm.ps1 is not digitally signed. You cannot run this script on
the current system. For more information about running scripts and setting execution policy, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ npm i -g @anthropic-ai/claude-code
+ ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Users\REDACTED.USER>
PS C:\Users\REDACTED.USER> # Verify the shim landed in the new prefix and resolves
PS C:\Users\REDACTED.USER> where.exe claude
INFO: Could not find files for the given pattern(s).
PS C:\Users\REDACTED.USER> claude --version
claude : The term 'claude' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ claude --version
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (claude:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\REDACTED.USER> # === end of CLEAN SETUP ===
PS C:\Users\REDACTED.USER>
