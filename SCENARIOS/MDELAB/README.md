Quick and dirty MDE tests

Labconfig for a MDE environment


```
# Domain controller

$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'mdelab-' ; DomainNetbiosName="mdelab";DomainName="mdelab.jve5dtest.com"; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$true ;AdditionalNetworksConfig=@(); MemoryStartupBytes= 2GB; VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

# Servers
$LabConfig.VMs += @{ VMName = 'mdetools'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew11'     ; Configuration = 'Simple'; ParentVHD = 'Win1122H2_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew10'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew11-2'     ; Configuration = 'Simple'; ParentVHD = 'Win1122H2_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew10-2'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew10-webf'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew10-avdisabled'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'mdew10-demo'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 6GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```
