Quick CA tests

Labconfig for CA lab

Tää vähän syö muistia, ainakin konfiguraation perusteella

| HOST | MEMORY |
| --- | --- |
| DC | 2GB |
| CA | 4GB |
| IIS | 3GB |
| WAC | 4GB |
| FILES | 3GB |
| *WKS* | **Ei vielä konfiguroitu** |

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'ca-' ;DomainNetbiosName="corp";DomainName="corp.ca.local"; DCEdition='4'; Internet=$true ; AdditionalNetworksConfig=@(); VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

# Servers
$LabConfig.VMs += @{ VMName = 'ca'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'iis'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 3GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wac'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'files'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 3GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
# $LabConfig.VMs += @{ VMName = 'wks'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 8GB ;AdditionalNetworks = $False; MGMTNICs = 1 }

```