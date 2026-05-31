# Terraform Notes – Class 23 (31-05-2026)

Based on the uploaded class notes and diagram. 

---

# Terraform Configuration Structure

Terraform configuration file consists of different blocks:

```hcl
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
    }
  }
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "babli" {
  name     = "banti-rg"
  location = "Central India"
}
```

---

# 1. Terraform Block

The `terraform {}` block is used to configure Terraform itself.

### Purpose

* Configure Terraform settings
* Define required providers
* Define provider source
* Configure backend settings

### Example

```hcl
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
    }
  }
}
```

---

# 2. Required Providers Block

Defines which provider Terraform needs.

### Example

```hcl
required_providers {
  azurerm = {
    source = "hashicorp/azurerm"
  }
}
```

### Benefits

* Downloads required provider plugins
* Ensures version compatibility
* Identifies provider source

---

# 3. Provider Block

Provider block is used to configure the cloud provider.

### Example

```hcl
provider "azurerm" {
  features {}
}
```

### Purpose

* Connect Terraform with Azure
* Authenticate with Azure
* Configure provider settings

---

# 4. Features Block

The `features {}` block is mandatory for AzureRM provider.

### Example

```hcl
provider "azurerm" {
  features {}
}
```

### Purpose

* Enables AzureRM provider features
* Initializes provider functionality

---

# 5. Resource Block

Resource block is used to create cloud resources.

### Syntax

```hcl
resource "<resource_type>" "<resource_name>" {
  arguments
}
```

### Example

```hcl
resource "azurerm_resource_group" "babli" {
  name     = "banti-rg"
  location = "Central India"
}
```

### Components

| Part                   | Description                 |
| ---------------------- | --------------------------- |
| resource               | Block Type                  |
| azurerm_resource_group | Resource Type               |
| babli                  | Local Name (Terraform Only) |
| name                   | Azure Resource Group Name   |
| location               | Azure Region                |

---

# Understanding Labels

### Label-1

```hcl
azurerm_resource_group
```

Defines which resource Terraform will create.

### Label-2

```hcl
babli
```

Local reference name used inside Terraform.

Example:

```hcl
azurerm_resource_group.babli.name
```

---

# Terraform Workflow

## Step 1: Terraform Init

```bash
terraform init
```

### What Happens?

* Downloads provider plugins
* Creates `.terraform` directory
* Creates `.terraform.lock.hcl`

### Files Created

```text
.terraform/
.terraform.lock.hcl
```

Azure Provider plugin:

```text
azurerm.exe
```

---

## Step 2: Terraform Format

```bash
terraform fmt
```

### Purpose

* Formats Terraform code
* Improves readability
* Maintains coding standards

---

## Step 3: Terraform Validate

```bash
terraform validate
```

### Purpose

* Checks syntax errors
* Verifies configuration correctness

---

## Step 4: Terraform Plan

```bash
terraform plan
```

### Purpose

* Shows what Terraform will create
* No actual changes are made

Example:

```text
+ Resource Group will be created
```

---

## Step 5: Terraform Apply

```bash
terraform apply
```

### Purpose

* Creates resources in Azure
* Executes planned changes

---

# What Happens During Terraform Apply?

Terraform follows this flow:

```text
Terraform Apply
        |
        v
Azure Gateway
        |
        v
Microsoft Entra ID
        |
        v
Access Token (JSON Format)
        |
        v
Azure Resource Manager (ARM)
        |
        +---- Microsoft.Compute
        |
        +---- Microsoft.Network
        |
        +---- Microsoft.Resources
```

### Detailed Flow

1. Terraform sends REST API requests.
2. Azure authenticates through Microsoft Entra ID.
3. Azure returns an Access Token.
4. Terraform uses the token to communicate with ARM.
5. ARM creates resources in Azure.

---

# Important Terraform Commands

| Command            | Purpose                |
| ------------------ | ---------------------- |
| terraform init     | Initialize project     |
| terraform fmt      | Format code            |
| terraform validate | Validate configuration |
| terraform plan     | Preview changes        |
| terraform apply    | Create resources       |
| terraform destroy  | Delete resources       |

---

# Attribute References

Terraform can reference values from other resources.

### Syntax

```hcl
resource_type.resource_name.attribute
```

### Example

```hcl
azurerm_resource_group.babli.name
```

Output:

```text
banti-rg
```

---

# Interview Questions

### What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool used to provision and manage cloud resources using configuration files.

### What is a Provider?

A provider is a plugin that allows Terraform to interact with cloud platforms such as Azure, AWS, and GCP.

### Why do we use `features {}` in AzureRM Provider?

The `features {}` block enables AzureRM provider functionality and is mandatory.

### Difference Between Resource Type and Resource Name?

* Resource Type → Defines what resource to create.
* Resource Name → Local identifier used by Terraform.

### What does Terraform Init do?

* Downloads provider plugins
* Creates `.terraform` directory
* Creates `.terraform.lock.hcl`

### What does Terraform Plan do?

Displays execution plan without creating resources.

### What does Terraform Apply do?

Creates or modifies infrastructure based on the configuration.

---

## Quick Revision

```text
terraform {}
     ↓
required_providers {}
     ↓
provider "azurerm" {}
     ↓
features {}
     ↓
resource {}
     ↓
terraform init
     ↓
terraform fmt
     ↓
terraform validate
     ↓
terraform plan
     ↓
terraform apply
```