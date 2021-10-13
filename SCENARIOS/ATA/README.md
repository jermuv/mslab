Quick ATA tests

Labconfig for ata lab

```
# Domain controller
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'SENTINEL-' ; DomainNetbiosName="ATA";DomainName="Corp.ATA.com"; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$true ;AdditionalNetworksConfig=@(); MemoryStartupBytes= 2GB; VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

# Servers
$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 2GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wef'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 2GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```