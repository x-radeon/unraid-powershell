# unraid-powershell
This is an Unraid plugin for Microsoft PowerShell (https://github.com/PowerShell/PowerShell).


The install is performed, basically, as described here: https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux

It probably needs some improvement.

Version will be lock step with the PowerShell stable version

To use PowerShell after install, type `pwsh` at a terminal or call `pwsh`  in a bash script (when using 'User Scripts" for example) like so: 
```
#!/bin/bash
pwsh /boot/config/plugins/myscripts/script.ps1
```
