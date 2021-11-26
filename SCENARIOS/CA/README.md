Quick CA tests

Labconfig for CA lab

Tää vähän syö muistia, ainakin konfiguraation perusteella

| HOST | MEMORY | ROLE |
| --- | --- | --- |
| DC | 2GB | SERVER |
| CA | 4GB | SERVER |
| IIS | 3GB | SERVER |
| WAC | 4GB | SERVER |
| FILES | 3GB | SERVER
| *WKS* | 8GB | WORKSTATION |

- Palvelimet: 16GB
- Työasema(t): 8GB
- Total: 24GB

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'ca-' ;DomainNetbiosName="corp";DomainName="corp.ca.local"; DCEdition='4'; Internet=$true ; AdditionalNetworksConfig=@(); VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

# Servers
$LabConfig.VMs += @{ VMName = 'ca'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'iis'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 3GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wac'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'files'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 3GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wks'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 8GB ;AdditionalNetworks = $False; MGMTNICs = 1 }

```