# Terraform Basics – Azure Resource Group Deployment

## Objective
Deploy an Azure Resource Group (RG) using Terraform.

---

# Introduction to Terraform

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to provision, manage, and automate infrastructure across multiple cloud providers such as Azure, AWS, and GCP.

## Advantages of Terraform

- Infrastructure as Code (IaC)
- Multi-Cloud Support
- Automation and Repeatability
- Version Control Integration
- Faster Infrastructure Deployment
- Reduced Human Errors

---

# Terraform Configuration File

Terraform code is stored in files with the `.tf` extension.

Example:

```hcl
main.tf
```

Terraform uses **HashiCorp Configuration Language (HCL)**.

---

# HCL (HashiCorp Configuration Language)

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "demo-rg"
  location = "Central India"
}
```

## HCL Components

| Component | Description |
|------------|-------------|
| Block | Enclosed within `{}` |
| Argument | Configuration input |
| Attribute | Resource output value |

---

# Azure CLI Workflow

## Install Azure CLI

```bash
az version
```

## Login to Azure

```bash
az login
```

## Display Help

```bash
az --help
```

---

# Terraform Workflow

## Step 1: Install Terraform

```bash
terraform version
```

## Step 2: Login to Azure

```bash
az login
```

## Step 3: Create Terraform Configuration

Create a file:

```text
main.tf
```

## Step 4: Initialize Terraform

```bash
terraform init
```

Purpose:
- Downloads providers
- Creates `.terraform` directory
- Initializes working directory

## Step 5: Format Code

```bash
terraform fmt
```

## Step 6: Validate Configuration

```bash
terraform validate
```

## Step 7: Generate Execution Plan

```bash
terraform plan
```

## Step 8: Apply Configuration

```bash
terraform apply
```

## Step 9: Destroy Resources

```bash
terraform destroy
```

---

# Terraform Architecture

```text
Terraform Code
      ↓
Terraform CLI
      ↓
AzureRM Provider
      ↓
Azure REST API
      ↓
Azure Resource Manager (ARM)
      ↓
Azure Resources
```

---

# Understanding Providers

A provider is a plugin that enables Terraform to communicate with cloud platforms.

| Cloud Platform | Provider |
|---------------|----------|
| Microsoft Azure | azurerm |
| Amazon AWS | aws |
| Google Cloud | google |

Example:

```hcl
provider "azurerm" {
  features {}
}
```

---

# Terraform Block

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.74.0"
    }
  }
}
```

Purpose:
- Defines Terraform settings
- Specifies required providers
- Controls provider versions

---

# Required Provider Block

```hcl
required_providers {
  azurerm = {
    source  = "hashicorp/azurerm"
    version = "~> 4.74.0"
  }
}
```

---

# Provider Block

```hcl
provider "azurerm" {
  features {}
}
```

Purpose:
- Connect Terraform with Azure
- Configure provider-specific settings

---

# Complete Azure Resource Group Example

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.74.0"
    }
  }
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "cloudtechhacks-rg"
  location = "Central India"
}
```

---

# REST API Fundamentals

A REST API enables communication between applications using HTTP protocols.

## REST API Components

| Component | Description |
|------------|------------|
| Endpoint | URL used for communication |
| HTTP Method | GET, POST, PUT, DELETE |
| Request Body | Data sent to API |
| Response Body | Data returned by API |

Example Endpoint:

```http
GET https://api.contoso.com/users
```

---

# HTTP Methods

| Method | Purpose |
|----------|---------|
| GET | Retrieve Data |
| POST | Create Data |
| PUT | Update/Create Data |
| DELETE | Remove Data |

---

# JSON Request Example

```json
{
  "name": "Jay",
  "location": "India"
}
```

---

# Azure Resource Manager (ARM)

ARM is the management layer of Microsoft Azure.

All Azure tools communicate with ARM:

- Azure Portal
- Azure CLI
- Azure PowerShell
- Terraform
- SDKs
- REST APIs

---

# Authentication Flow

```text
User
 ↓
Azure Portal
 ↓
Microsoft Entra ID
 ↓
JWT Token
 ↓
Azure Resource Manager
 ↓
Azure Resource
```

Terraform follows a similar flow:

```text
Terraform
 ↓
AzureRM Provider
 ↓
ARM REST API
 ↓
Azure Resources
```

---

# Interview Questions

## What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool used to automate infrastructure deployment.

## What language does Terraform use?
HashiCorp Configuration Language (HCL).

## What is a Provider?
A provider is a plugin that enables Terraform to communicate with a cloud platform.

## What is AzureRM Provider?
The official Terraform provider for Azure resources.

## What does Terraform Init do?
Initializes the working directory and downloads providers.

## Difference Between Plan and Apply

| Terraform Plan | Terraform Apply |
|---------------|------------------|
| Shows Changes | Executes Changes |
| No Resources Created | Resources Created |

## Which Azure service receives Terraform requests?
Azure Resource Manager (ARM).

---

# Quick Revision

```text
Terraform File → main.tf

terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy

Provider → azurerm

Language → HCL

Authentication → az login
```
