# Terraform Remote Backend (`azurerm`) – Complete Notes

## What is a Terraform Backend?

A backend in Terraform determines **where Terraform stores its state file (`terraform.tfstate`)**.

By default, Terraform uses the **local backend**, which stores the state file on your local machine.

**Example:**

```text
terraform.tfstate
```

### Problems with Local Backend

* State file can be deleted accidentally.
* No collaboration between team members.
* No centralized storage.
* No state locking.
* Difficult to use in production.

For production environments, always use a **Remote Backend**.

---

# What is AzureRM Backend?

The `azurerm` backend stores Terraform state files inside an **Azure Storage Account Blob Container**.

```text
Terraform State
        │
        ▼
Azure Storage Account
        │
        ▼
Blob Container
        │
        ▼
terraform.tfstate
```

This provides:

* Centralized state management
* Team collaboration
* Security
* High availability
* State locking
* Versioning support

---

# Prerequisites

Before configuring the AzureRM backend, create:

* Azure Subscription
* Resource Group
* Storage Account
* Blob Container

**Example:**

```text
Resource Group
    terraform-rg

Storage Account
    tfstate12345

Container
    tfstate

Blob
    terraform.tfstate
```

---

# Create Resource Group

```bash
az group create \
  --name terraform-rg \
  --location centralindia
```

---

# Create Storage Account

```bash
az storage account create \
  --resource-group terraform-rg \
  --name tfstate12345 \
  --sku Standard_LRS \
  --kind StorageV2
```

---

# Create Blob Container

```bash
az storage container create \
  --name tfstate \
  --account-name tfstate12345 \
  --auth-mode login
```

---

# Backend Configuration

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-rg"
    storage_account_name = "tfstate12345"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

---

# Parameter Explanation

## `resource_group_name`

The Resource Group containing the Storage Account.

Example:

```text
terraform-rg
```

## `storage_account_name`

The Azure Storage Account name.

Example:

```text
tfstate12345
```

## `container_name`

The Blob Container where the state file will be stored.

Example:

```text
tfstate
```

## `key`

The state file name (or path).

Examples:

```text
terraform.tfstate
prod.tfstate
dev.tfstate
```

---

# Initialize Backend

Run:

```bash
terraform init
```

Expected output:

```text
Initializing the backend...

Successfully configured the backend "azurerm".
```

Terraform downloads the backend configuration and connects to Azure Storage.

---

# Authentication Methods

## 1. Azure CLI (Recommended)

Login:

```bash
az login
```

Terraform automatically uses Azure CLI credentials.

---

## 2. Service Principal

Create:

```bash
az ad sp create-for-rbac
```

Set environment variables:

```bash
export ARM_CLIENT_ID="xxxxxxxx"
export ARM_CLIENT_SECRET="xxxxxxxx"
export ARM_SUBSCRIPTION_ID="xxxxxxxx"
export ARM_TENANT_ID="xxxxxxxx"
```

Terraform automatically uses these variables.

---

## 3. Managed Identity

Used in:

* Azure Virtual Machines
* Azure Virtual Machine Scale Sets (VMSS)
* Azure Kubernetes Service (AKS)
* Azure DevOps Self-hosted Agents

No secrets are required.

---

# Complete Example

```hcl
terraform {

  backend "azurerm" {
    resource_group_name  = "terraform-rg"
    storage_account_name = "tfstate12345"
    container_name       = "tfstate"
    key                  = "dev/terraform.tfstate"
  }

}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "demo" {
  name     = "demo-rg"
  location = "Central India"
}
```

---

# Backend Migration

Initially using a local backend:

```text
terraform.tfstate
```

After configuring the remote backend:

```bash
terraform init
```

Terraform prompts:

```text
Do you want to copy existing state to the new backend?
```

Answer:

```text
yes
```

Terraform uploads the local state file to Azure Blob Storage.

---

# Verify the State File

From the Azure Portal:

```text
Storage Account
      │
      ▼
Containers
      │
      ▼
tfstate
      │
      ▼
terraform.tfstate
```

Or using Azure CLI:

```bash
az storage blob list \
  --account-name tfstate12345 \
  --container-name tfstate \
  --auth-mode login
```

---

# State Locking

AzureRM backend supports state locking.

```text
Developer A
terraform apply
        │
        ▼
Lock Acquired

Developer B
terraform apply
        │
        ▼
Error:
State already locked.
```

This prevents concurrent modifications and state corruption.

---

# Multiple Environment Structure

```text
Storage Account
│
├── dev
│     └── terraform.tfstate
│
├── qa
│     └── terraform.tfstate
│
└── prod
      └── terraform.tfstate
```

Example backend:

```hcl
backend "azurerm" {
  resource_group_name  = "terraform-rg"
  storage_account_name = "tfstate12345"
  container_name       = "tfstate"
  key                  = "prod/terraform.tfstate"
}
```

---

# Security Best Practices

* Enable Storage Account encryption.
* Disable public access.
* Use Azure RBAC instead of Storage Account keys.
* Enable Blob Versioning.
* Enable Soft Delete.
* Use Private Endpoints when required.
* Store credentials securely.
* Never commit secrets to GitHub.

---

# Advantages of AzureRM Backend

* Centralized state storage
* Team collaboration
* Secure storage
* Automatic state locking
* Backup support
* Blob versioning
* High availability
* Disaster recovery support
* Production-ready
* CI/CD integration

---

# Local Backend vs AzureRM Backend

| Local Backend                  | AzureRM Backend                    |
| ------------------------------ | ---------------------------------- |
| Stores state locally           | Stores state in Azure Blob Storage |
| Single-user friendly           | Team collaboration                 |
| No state locking               | Supports state locking             |
| Easy to lose                   | Highly durable                     |
| Not recommended for production | Production-ready                   |
| Manual backups                 | Azure-managed backup options       |
| No centralized management      | Centralized state management       |

---

# Real-World Architecture

```text
                Developer
                     │
             terraform apply
                     │
                     ▼
          Azure Authentication
                     │
                     ▼
      AzureRM Backend (Blob Storage)
                     │
             terraform.tfstate
                     │
                     ▼
      Azure Resource Manager (ARM)
                     │
      ┌─────────┬─────────┬─────────┐
      ▼         ▼         ▼         ▼
     VM       VNet       NSG   Resource Group
```

---

# Interview Questions

## Q1. What is a Terraform backend?

A backend defines where Terraform stores and manages its state file (`terraform.tfstate`).

---

## Q2. Why should we use a remote backend?

To enable centralized state storage, team collaboration, state locking, and improved reliability.

---

## Q3. Where does the AzureRM backend store the state file?

In an Azure Blob Storage container inside an Azure Storage Account.

---

## Q4. What is the purpose of the `key` parameter?

It specifies the name and path of the state file inside the Blob container.

---

## Q5. What happens when `terraform init` is run after configuring a remote backend?

Terraform initializes the backend and can migrate the existing local state to the remote backend after user confirmation.

---

## Q6. Which authentication methods are supported?

* Azure CLI (`az login`)
* Service Principal
* Managed Identity

---

## Q7. Why is state locking important?

It prevents multiple users from modifying the same state simultaneously and avoids state corruption.

---

## Q8. Can multiple environments use the same Storage Account?

Yes. Separate state files can be maintained using different `key` values, such as:

```text
dev/terraform.tfstate
qa/terraform.tfstate
prod/terraform.tfstate
```

---

## Q9. Is the local backend recommended for production?

No. A remote backend like `azurerm` should be used for production deployments.

---

## Q10. What are some security best practices for the AzureRM backend?

* Enable encryption.
* Use Azure RBAC.
* Disable public access.
* Enable Blob Versioning and Soft Delete.
* Never store credentials in source code repositories.
