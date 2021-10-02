Quick ATA tests

Labconfig for ata lab

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'ATA-' ; DomainNetbiosName="ATA";DomainName="Corp.ATA.com"; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$true ;AdditionalNetworksConfig=@(); MemoryStartupBytes= 6GB; VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 2GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'atacenter'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```