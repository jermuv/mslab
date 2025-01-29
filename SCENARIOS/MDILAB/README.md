# Quick and dirty MDI tests

Labconfig for a MDI environment

```
# Domain controller
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'mdilab-' ; DomainNetbiosName="mdilab";DomainName="mdilab.jve5dtest.com"; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$true ;AdditionalNetworksConfig=@(); MemoryStartupBytes= 8GB; StaticMemory= $true; VMs=@()}
# DCEdition = 4 for DataCenter or 3 for DataCenterCore
# Servers
$LabConfig.VMs += @{ VMName = 'mditools'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdiw11'     ; Configuration = 'Simple'; ParentVHD = 'Win1124H2_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```

# Installing MDI

## Enable recycle bin

- ADAC -> enable

## Install sensor

- download (or have from 3. resources -> jve5dtest xdr onboarding...) -> install

## Prepare the service accounts

Take a look for the 3 scripts:

- MDI DSA creation (mdidsacreate.ps1)
- MDI AA creation (mdiaacreate.ps1)
- MDI Deleted objects permissions (mdideletedperms.ps1)

**mdidsacreate.ps1:**
```
# Variables:
# Specify the name of the gMSA you want to create:
$gMSA_AccountName = 'svcMDIAction'
# Specify the name of the group you want to create for the gMSA,
# or enter 'Domain Controllers' to use the built-in group when your environment is a single forest, and will contain only domain controller sensors.
$gMSA_HostsGroupName = 'Domain Controllers'
# Specify the computer accounts that will become members of the gMSA group and have permission to use the gMSA.
# If you are using the 'Domain Controllers' group in the $gMSA_HostsGroupName variable, then this list is ignored
$gMSA_HostNames = 'DC'

# Import the required PowerShell module:
Import-Module ActiveDirectory

# Set the group
if ($gMSA_HostsGroupName -eq 'Domain Controllers') {
    $gMSA_HostsGroup = Get-ADGroup -Identity 'Domain Controllers'
} else {
    $gMSA_HostsGroup = New-ADGroup -Name $gMSA_HostsGroupName -GroupScope DomainLocal -PassThru
    $gMSA_HostNames | ForEach-Object { Get-ADComputer -Identity $_ } |
        ForEach-Object { Add-ADGroupMember -Identity $gMSA_HostsGroupName -Members $_ }
}

# Create the gMSA:
New-ADServiceAccount -Name $gMSA_AccountName -DNSHostName "$gMSA_AccountName.$env:USERDNSDOMAIN" -PrincipalsAllowedToRetrieveManagedPassword $gMSA_HostsGroup
```


## Install AD + GPO tools for mditools

- Get-WindowsFeature 
- Add-WindowsFeature RSAT-AD-Tools
- Add-WindowsFeature GPMC

## Download powershell module

Do this on both mditools and dc machines

- Install-Module -Name DefenderForIdentity

## Enable auditing and other important things

## Deleted objects container



# Links for the resources

- Official [MDI documentation](https://learn.microsoft.com/en-us/defender-for-identity/)
- MDI [Ninja training](http://aka.ms/mdininja)
- MDI [PowerShell module](https://www.powershellgallery.com/packages/DefenderForIdentity)

