Quick CA tests

Labconfig for CA lab

Tää vähän syö muistia, ainakin konfiguraation perusteella

| HOST | MEMORY | ROLE |
| --- | --- | --- |
| DC | 2GB | SERVER |
| SUBCA | 4GB | SERVER |
| ROOTCA | 4GB | (MEMBER) SERVER |
| IIS | 3GB | SERVER |
| *WKS* | 8GB | WORKSTATION |

- Palvelimet: 13GB
- Työasema(t): 8GB
- Total: 21GB

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'ca-' ;DomainNetbiosName="corp";DomainName="corp.ca.local"; DCEdition='4'; Internet=$true ; AdditionalNetworksConfig=@(); VMs=@()}

# DCEdition = 4 for DataCenter or 3 for DataCenterCore

# Servers
$LabConfig.VMs += @{ VMName = 'subca'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'rootca'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 ; Unattend="NoDjoin"}
$LabConfig.VMs += @{ VMName = 'iis'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 3GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wks'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 8GB ;AdditionalNetworks = $False; MGMTNICs = 1 }

```