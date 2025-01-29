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

# Links for the documentation

- Official [Documentation](https://learn.microsoft.com/en-us/defender-for-identity/)
- MDI [Ninja training](http://aka.ms/mdininja)

