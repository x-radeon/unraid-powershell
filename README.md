# unraid-powershell
This is an Unraid plugin for Microsoft PowerShell (https://github.com/PowerShell/PowerShell).

The install is performed, basically, as described [here](https://learn.microsoft.com/en-us/powershell/scripting/install/install-other-linux?view=powershell-7.4#installation-using-a-binary-archive-file).

It probably needs some improvement.

## Powershell version

Version will be lock step with the PowerShell stable version

To use PowerShell after install, type `pwsh` at a terminal or call `pwsh`  in a bash script (when using 'User Scripts" for example) like so: 
```
#!/bin/bash
pwsh /boot/config/plugins/myscripts/script.ps1
```

## Steps To Increment Plugin Version
1. View PowerShell versions at https://github.com/PowerShell/powershell/releases
2. Change version (line 5) to new version
3. Update SHA256 hash (line 6) to new hash (use hash for powershell-X.X.X-linux-x64.tar.gz)
4. Add info in the <CHANGES> section (line 15) simliar to previous changes


## Automatically Run scripts on startup/install

If you place any ps1 files into the plugin folder (`/boot/config/plugins/my_startup_scripts`), then they will be executed once the plugin install has been done. 

See [here](./startupScriptsExample.md) for an example

__Considerations__

* The filenames are sorted alphabetically, so if you want to run multiple files, make sure you name them appropriately.
* The scripts are going to be running without any visibility, so you need to ensure that they aren't going to be prompting you for any reason
* *Make sure you only install modules from sources that you trust!*
* Don't just blindly copy/paste scripts from the internet without understanding what they're doing
