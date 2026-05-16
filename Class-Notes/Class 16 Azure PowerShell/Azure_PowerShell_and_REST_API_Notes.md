# Azure PowerShell & Azure REST API Notes

# 1. What is Azure PowerShell?

Azure PowerShell ek command-line tool hai jo PowerShell ke through Azure resources ko manage karne ke liye use hota hai.

- Azure resource create kar sakte ho
- VM manage kar sakte ho
- Storage handle kar sakte ho
- Networking configure kar sakte ho
- Automation scripts likh sakte ho

Azure PowerShell mainly **Az Module** use karta hai.

---

# 2. Azure PowerShell Installation

## Install Az Module

```powershell
Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
```

## Check Installed Version

```powershell
Get-InstalledModule -Name Az
```

## Update Az Module

```powershell
Update-Module -Name Az
```

---

# 3. Login to Azure

## Login Command

```powershell
Connect-AzAccount
```

Browser open hoga aur authentication hogi.

---

# 4. Azure Subscription Commands

## Show Subscriptions

```powershell
Get-AzSubscription
```

## Select Subscription

```powershell
Set-AzContext -SubscriptionId "subscription-id"
```

## Check Current Context

```powershell
Get-AzContext
```

---

# 5. Resource Group Commands

## Create Resource Group

```powershell
New-AzResourceGroup -Name my-rg -Location eastus
```

## Get Resource Groups

```powershell
Get-AzResourceGroup
```

## Delete Resource Group

```powershell
Remove-AzResourceGroup -Name my-rg
```

---

# 6. Virtual Machine Commands

## Create VM

```powershell
New-AzVm `
-ResourceGroupName my-rg `
-Name myvm `
-Location eastus `
-VirtualNetworkName myvnet `
-SubnetName mysubnet `
-SecurityGroupName mynsg `
-PublicIpAddressName myip `
-OpenPorts 3389
```

## Start VM

```powershell
Start-AzVM -ResourceGroupName my-rg -Name myvm
```

## Stop VM

```powershell
Stop-AzVM -ResourceGroupName my-rg -Name myvm
```

## Restart VM

```powershell
Restart-AzVM -ResourceGroupName my-rg -Name myvm
```

## Get VM Status

```powershell
Get-AzVM -ResourceGroupName my-rg -Status
```

---

# 7. Storage Account Commands

## Create Storage Account

```powershell
New-AzStorageAccount `
-ResourceGroupName my-rg `
-Name mystorage123 `
-Location eastus `
-SkuName Standard_LRS
```

## Get Storage Accounts

```powershell
Get-AzStorageAccount
```

---

# 8. Virtual Network Commands

## Create VNet

```powershell
New-AzVirtualNetwork `
-Name myvnet `
-ResourceGroupName my-rg `
-Location eastus `
-AddressPrefix 10.0.0.0/16
```

---

# 9. NSG Commands

## Create NSG

```powershell
New-AzNetworkSecurityGroup `
-ResourceGroupName my-rg `
-Location eastus `
-Name myNSG
```

---

# 10. Helpful PowerShell Commands

## Find Commands

```powershell
Get-Command *AzVM*
```

## Get Help

```powershell
Get-Help Get-AzVM
```

## Detailed Help

```powershell
Get-Help Get-AzVM -Detailed
```

## Examples

```powershell
Get-Help Get-AzVM -Examples
```

## Online Documentation

```powershell
Get-Help Get-AzVM -Online
```

---

# 11. Azure PowerShell Pipeline Example

```powershell
Get-AzVM | Stop-AzVM
```

---

# 12. Variables in PowerShell

```powershell
$rg = "my-rg"

Get-AzResourceGroup -Name $rg
```

---

# 13. Loop Example

```powershell
$vmList = Get-AzVM

foreach($vm in $vmList)
{
    $vm.Name
}
```

---

# 14. Azure REST API Introduction

Azure REST API ek HTTP-based service hai jiske through directly Azure resources manage kiye ja sakte hain.

Methods:

- GET → Read Resource
- POST → Create Resource
- PUT → Update Resource
- DELETE → Remove Resource

---

# 15. REST API Authentication

## Get Access Token using PowerShell

```powershell
Get-AzAccessToken
```

---

# 16. Basic REST API URL Structure

```text
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{rg-name}?api-version=2021-04-01
```

---

# 17. Call Azure REST API using PowerShell

## Example: Get Resource Groups

```powershell
$token = (Get-AzAccessToken).Token

$headers = @{
    Authorization = "Bearer $token"
}

Invoke-RestMethod `
-Uri "https://management.azure.com/subscriptions/{subscription-id}/resourcegroups?api-version=2021-04-01" `
-Headers $headers `
-Method GET
```

---

# 18. Difference Between Azure CLI, PowerShell & REST API

| Feature | Azure CLI | Azure PowerShell | REST API |
|---|---|---|---|
| Language | Bash Style | PowerShell Style | HTTP |
| Output | JSON | Objects | JSON |
| Automation | Good | Excellent | Advanced |

---

# 19. Important Interview Questions

## Azure PowerShell

1. What is Az Module?
2. Difference between Azure CLI and Azure PowerShell?
3. What is Connect-AzAccount?
4. How to switch subscription?
5. What is PowerShell pipeline?

## Azure REST API

1. What is REST API?
2. What is ARM?
3. What is API Version?
4. How authentication works?
5. Difference between PUT and POST?

---

# 20. Important URLs

## Azure PowerShell Documentation

https://learn.microsoft.com/powershell/azure/

## Azure REST API Documentation

https://learn.microsoft.com/rest/api/azure/
