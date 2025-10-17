Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
 
At C:\Users\d-david.m.golub\OneDrive - afs.com\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1:3 char:361

+ ... \Roaming\npm = (\C:\Users\d-david.m.golub\Tools\npm-global-22;C:\User ...

+                                                                  ~

Missing closing ')' in expression.

At C:\Users\d-david.m.golub\OneDrive - afs.com\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1:3 char:716

+ ... \ -and \ -notmatch '\\Program Files\\nodejs|\\WindowsApps' }) -join ' ...

+                                                                 ~

Unexpected token ')' in expression or statement.

    + CategoryInfo          : ParserError: (:) [], ParseException

    + FullyQualifiedErrorId : MissingEndParenthesisInExpression
 
PS C:\Users\d-david.m.golub> claude
& : The term 'node.exe' is not recognized as the name of a cmdlet, function, script file, or operable program. Check

the spelling of the name, or if a path was included, verify that the path is correct and try again.

At C:\Users\d-david.m.golub\AppData\Roaming\npm\claude.ps1:24 char:7

+     & "node$exe"  "$basedir/node_modules/@anthropic-ai/claude-code/cl ...

+       ~~~~~~~~~~

    + CategoryInfo          : ObjectNotFound: (node.exe:String) [], CommandNotFoundException

    + FullyQualifiedErrorId : CommandNotFoundException
 
