# Azure CLI Notes

# What is Azure CLI?

Azure CLI (Command Line Interface) ek command-line tool hai jiska use Azure resources ko manage karne ke liye kiya jata hai.

Iske through hum:

- Virtual Machines create kar sakte hain
- Storage manage kar sakte hain
- Networking configure kar sakte hain
- Resource Groups create kar sakte hain
- Automation perform kar sakte hain
- Scripts execute kar sakte hain

Azure CLI Linux, Windows aur macOS sabhi operating systems par run karta hai.

---

# Why Use Azure CLI?

## Advantages

- Fast deployment
- Automation support
- Infrastructure as Code friendly
- Reusable scripts
- DevOps integration
- Easy resource management
- Time saving
- Bulk operations possible

---

# Azure CLI Architecture

User → Azure CLI Command → Azure REST API → Azure Resources

Example:

```bash
az vm create
```

Yeh command Azure API ko call karti hai aur VM create karti hai.

---

# Install Azure CLI

## Windows

Download:
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows

## Linux

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

## macOS

```bash
brew update && brew install azure-cli
```

---

# Verify Azure CLI Installation

```bash
az version
```

---

# Login to Azure

## Interactive Login

```bash
az login
```

---

# Resource Group Commands

## Create Resource Group

```bash
az group create --name MyRG --location centralindia
```

## List Resource Groups

```bash
az group list --output table
```

## Delete Resource Group

```bash
az group delete --name MyRG
```

---

# Virtual Machine Commands

## Create VM

```bash
az vm create \
--resource-group MyRG \
--name MyVM \
--image Ubuntu2204 \
--admin-username azureuser \
--generate-ssh-keys
```

## Start VM

```bash
az vm start --name MyVM --resource-group MyRG
```

## Stop VM

```bash
az vm stop --name MyVM --resource-group MyRG
```

---

# Storage Account Commands

## Create Storage Account

```bash
az storage account create \
--name mystorage123 \
--resource-group MyRG \
--location centralindia \
--sku Standard_LRS
```

---

# Networking Commands

## Create VNet

```bash
az network vnet create \
--resource-group MyRG \
--name MyVNet \
--address-prefix 10.0.0.0/16 \
--subnet-name MySubnet \
--subnet-prefix 10.0.1.0/24
```

---

# Azure CLI Help Commands

```bash
az --help
```

```bash
az vm --help
```

---

# Useful Commands

## Check Current Subscription

```bash
az account show
```

## List Locations

```bash
az account list-locations --output table
```

---

# Azure CLI Real-Time Use Cases

- CI/CD automation
- Infrastructure deployment
- VM management
- Storage management
- Network management
- Backup automation

---

# Best Practices

- Use naming conventions
- Store scripts in Git
- Use variables in scripts
- Avoid hardcoded passwords
- Test commands in non-production environment

---

# Difference Between Azure CLI and PowerShell

| Azure CLI | Azure PowerShell |
|---|---|
| Bash style | PowerShell style |
| Cross-platform | PowerShell focused |
| az commands | Verb-Noun commands |

---

# Interview Questions

## What is Azure CLI?

Azure resources ko command line se manage karne ka tool.

## How to login in Azure CLI?

```bash
az login
```

## How to create Resource Group?

```bash
az group create --name MyRG --location centralindia
```

---

# Conclusion

Azure CLI ek powerful automation tool hai jo Azure resource management ko fast, reusable aur scalable banata hai.

DevOps, Cloud Administration aur Automation me Azure CLI bahut important role play karta hai.
