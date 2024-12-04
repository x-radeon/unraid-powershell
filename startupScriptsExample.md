[← README](./README.md)

# Retrieve unraid keyfile from an Azure Keyvault

For example, this set of files (all in `/boot/config/plugins/PowerShell/`) will 
* install some modules for Azure Keyvault manipulation
* retrieve the value for /root/keyfile from the KV and write it

## `00.trust.ps1`

Set the PSGallery repo to be Trusted so that we don't get prompted when installing modules from there

``` powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## `01.installModule.ps1`

Install required modules 

``` powershell
Install-Module -Name Az.Accounts -AllowClobber -Force
Install-Module -Name Az.KeyVault -AllowClobber -Force
```

## `02.getKeyfile.ps1`

Get the secret value from an Azure keyvault. 

The thinking behind this is,
* If the keyfile is saved on the USB drive (as is the default practice), then if someone knocks-off my whole server then they can decrypt my drives and access any of my data once they plug it back in again
* But not if the keyfile isn't actually stored on the server at all
* On boot, connect to Az Keyvault, retrive the keyfile value and write it to /root/keyfile
* Before anyone gets excited and tells me that I shouldn't have the client secret etc. hard-coded in here because it makes the whole keyvault exercise pointless, i'm actually (marginally) smarter than that.
  * the Keyvault has a firewall set up on it in Azure such that it can only be accessed from certain locations; so essentially, if they are not on my LAN then they won't be able to access the KV to get the keyfile so they can't decrypt the drives

Issues/Limitations with this method
 * If then internet is down, then I cannot retrieve the key - so can't unlock the drives. That being said, the internet only needs to be up while the script is running (as once the keyfile is in /root/ then all is good in the world)
 * Keyvault firewall rules need to be maintained - if you've got it set up to only allow access for a given (public) IP, if your ISP changes your IP then you'll need to nip into Azure to change the config
 * Service Principal Secret value - they expire, so you need to keep on top of that.

``` powershell
#!/usr/bin/pwsh

$ErrorActionPreference = 'Stop'

$KeyVaultName = 'MY-KEYVAULT-NAME'
$SecretName = 'unraid' # name of the secret in the KV
$TenantId = 'a1b2c3d4-e5f6-7890-ab12-cd34ef567890'
$ApplicationId = '12345678-90ab-cdef-1234-567890abcdef' # Service Principal ID
$clientSecret = 'P@ssw0rd!23$%^&*()_+AbCdEfGhIjKlMnOpQrStUvWxYz1234567890' | ConvertTo-SecureString -AsPlainText -Force
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $ApplicationId, $clientSecret

# Connect to Azure account
Connect-AzAccount -ServicePrincipal -Tenant $TenantId -Credential $Credential *> $null

# Retrieve a secret from the Key Vault
# Get the secret with error handling
try {
    $secret = Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $SecretName
} catch {
    Write-Host "Error retrieving the secret: $_"
    Disconnect-AzAccount *> $null
    exit 1
}

# Check if the secret was retrieved successfully
if ($null -ne $secret -and $null -ne $secret.Id -and $null -ne $secret.Name) {
    Write-Host "Secret retrieved successfully."
    $keyfile="/root/keyfile"
    Write-Host "Writing secret to $keyfile"
    Set-Content -Path $keyfile -Value (ConvertFrom-SecureString -SecureString $secret.SecretValue -AsPlainText) -NoNewline
    chmod 600 $keyfile
} else {
    Write-Host "Failed to retrieve the secret."
}

Disconnect-AzAccount *> $null
```


[← README](./README.md)