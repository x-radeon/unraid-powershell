# unraid-powershell
This is an Unraid plugin for Microsoft PowerShell (https://github.com/PowerShell/PowerShell).


The install is performed, basically, as described [here](https://learn.microsoft.com/en-us/powershell/scripting/install/install-other-linux?view=powershell-7.4#installation-using-a-binary-archive-file).

It probably needs some improvement.

Version will be lock step with the PowerShell stable version

To use PowerShell after install, type `pwsh` at a terminal or call `pwsh`  in a bash script (when using 'User Scripts" for example) like so: 
```
#!/bin/bash
pwsh /boot/config/plugins/myscripts/script.ps1
```

## Steps To Increment Plugin Version
1. View PowerShell versions at https://github.com/PowerShell/powershell/releases
2. Change version (line 5) to new version
3. Update SHA256 hash (line 6) to new hash
  - Use hash for powershell-X.X.X-linux-x64.tar.gz
4. Add info in the <CHANGES> section (line 15) simliar to previous changes
