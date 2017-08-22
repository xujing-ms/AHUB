# AHUB - Azure Hybrid Use Benefit
Provides ability for customers to use their existing (or future) on-premises Windows Server licenses with software assurance on Azure to save on the cost of Windows licensing on Azure VMs. 

> [!NOTE]
> Special subscription access is required to run below scripts. See official documentation for current supported capabilities [Azure Hybrid Use Benefit for Windows Server](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing)
>
>

## PowerShell
### Convert to using AHUB
```powershell
		$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
		$vm.LicenseType = "Windows_Server"
		Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### Convert back to pay as you go
```powershell
		$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
		$vm.LicenseType = "None"
		Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```
## Azure CLI

```
az vm update -n <vm-name> -g <rg-name> --set LicenseType="Windows_Server"
```
## Portal
*** Coming future ***

## ARM Template
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2017-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "<vm-name>",
            "location": "[resourceGroup().location]",
            "properties": {
                "licenseType": "Windows_Server"
            }
        }
    ]
}
```

## REST API

```json
PUT
https://management.azure.com/subscriptions/<subId>/resourceGroups/<rg-name>/providers/Microsoft.Compute/virtualMachin
es/<vm-nam>?api-version=2016-04-30-preview

Headers:
x-ms-client-request-id        : <>
accept-language               : en-US
```
Body:
```json
{
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_A1"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "MicrosoftWindowsServer",
        "offer": "WindowsServer",
        "sku": "2012-R2-Datacenter",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Windows",
        "name": "<disk-name>",
        "caching": "ReadWrite",
        "createOption": "fromImage",
        "diskSizeGB": 128,
        "managedDisk": {
          "storageAccountType": "Standard_LRS",
          "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Compute/disks/<disk-name>"
        }
      },
      "dataDisks": []
    },
    "osProfile": {
      "computerName": "<vm-name>",
      "adminUsername": "<username>",
      "windowsConfiguration": {
        "provisionVMAgent": true,
        "enableAutomaticUpdates": true
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/<subId>/resourceGroups/<rg-name>/providers/Microsoft.Network/networkInterfaces/<nic>"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://<storagename>.blob.core.windows.net/"
      }
    },
    "licenseType": "Windows_Server"
  },
  "location": "<region>",
  "tags": {}
}
```

