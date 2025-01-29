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

## Prepare AD for GMSA

```
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))
```

## Prepare the service accounts

Take a look for the 3 scripts:

- MDI DSA creation (mdidsacreate.ps1)
- MDI AA creation (mdiaacreate.ps1)
- MDI Deleted objects permissions (mdideletedperms.ps1)

**mdidsacreate.ps1:**
```
# Variables:
# Specify the name of the gMSA you want to create:
$gMSA_AccountName = 'svcMDIService'
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

**mdiaacreate.ps1**

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

**mdideletedperms.ps1**

```
# Declare the identity that you want to add read access to the deleted objects container:
$Identity = 'svcMDIService'

# If the identity is a gMSA, first to create a group and add the gMSA to it:
$groupName = 'MDIDSA'
$groupDescription = 'Members of this group are allowed to read the objects in the Deleted Objects container in AD'
if(Get-ADServiceAccount -Identity $Identity -ErrorAction SilentlyContinue) {
    $groupParams = @{
        Name           = $groupName
        SamAccountName = $groupName
        DisplayName    = $groupName
        GroupCategory  = 'Security'
        GroupScope     = 'Universal'
        Description    = $groupDescription
    }
    $group = New-ADGroup @groupParams -PassThru
    Add-ADGroupMember -Identity $group -Members ('{0}$' -f $Identity)
    $Identity = $group.Name
}

# Get the deleted objects container's distinguished name:
$distinguishedName = ([adsi]'').distinguishedName.Value
$deletedObjectsDN = 'CN=Deleted Objects,{0}' -f $distinguishedName

# Take ownership on the deleted objects container:
$params = @("$deletedObjectsDN", '/takeOwnership')
C:\Windows\System32\dsacls.exe $params

# Grant the 'List Contents' and 'Read Property' permissions to the user or group:
$params = @("$deletedObjectsDN", '/G', ('{0}\{1}:LCRP' -f ([adsi]'').name.Value, $Identity))
C:\Windows\System32\dsacls.exe $params
  
# To remove the permissions, uncomment the next 2 lines and run them instead of the two prior ones:
# $params = @("$deletedObjectsDN", '/R', ('{0}\{1}' -f ([adsi]'').name.Value, $Identity))
# C:\Windows\System32\dsacls.exe $params

```


## Install AD + GPO tools for mditools

- Get-WindowsFeature 
- Add-WindowsFeature RSAT-AD-Tools
- Add-WindowsFeature GPMC

## Download powershell module

Do this on both mditools and dc machines

- Install-Module -Name DefenderForIdentity

## Enable auditing and other important things

**Powershell**

```
import-module defenderforidentity

# This example sets all configurations for the domain, creating the GPOs and linking them.
Set-MDIConfiguration -Mode Domain -Configuration All -Identity svcMDIService

# With this example, linking does not occur
# Set-MDIConfiguration -Mode Domain -Configuration All -GpoNamePrefix 'CONTOSO' -SkipGpoLink
-Identity svcMDIService
```

- **Note 1** - remote SAM gpo linking is not done anyway!
- **Note 2** - firewall NNR exception 

## Delegate accessrights for the action account

- [Link for the references](https://learn.microsoft.com/en-us/defender-for-identity/deploy/manage-action-accounts)

Permissions to give (ALL Descendant User objects)
- pwdlastset read / write
- useraccountcontrol read / write

## Deleted objects container

To verify deleted objects container, following scripts can be used:

**Checkdeleted.ps1**
```
$delObjContainer = Get-ADObject -Filter 'objectClass -eq "container"' -SearchBase (Get-ADDomain).DeletedObjectsContainer -Properties nTSecurityDescriptor -IncludeDeletedObjects -SearchScope base
$delObjContainer.nTSecurityDescriptor.Access

```

## Configurations on Defender for Identity side

- Configure MDI to use the newly created service account
- Configure MDI to use the newly created action account

# Links for the resources

- Official [MDI documentation](https://learn.microsoft.com/en-us/defender-for-identity/)
- MDI [Ninja training](http://aka.ms/mdininja)
- MDI [PowerShell module](https://www.powershellgallery.com/packages/DefenderForIdentity)

