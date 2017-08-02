# AHUB - Azure Hybrid Use Benefit
Provides ability for customers to use their existing (or future) on-premises Windows Server licenses with software assurance on Azure to save on the cost of Windows licensing on Azure VMs. 

>[!IMPORTANT]
>Special subscrption access is required to run below scripts. See official documentation for current supported capabilities [Azure Hybrid Use Benefit for Windows Server](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing)
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
		$vm.LicenseType = ""
		Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```
