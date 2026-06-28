# Terraform Modules - Complete Notes

> **Class 32 – Terraform Modules**

---

# What is a Terraform Module?

A **Terraform Module** is a collection of Terraform configuration files (`.tf`) grouped together to perform a specific task.

Instead of writing the same infrastructure code multiple times, we write it once inside a module and reuse it wherever required.

## Definition

> **A Terraform Module is a reusable collection of Terraform configuration files that provision one or more infrastructure resources.**

---

# Why Do We Need Modules?

Consider an organization with three environments:

* Development
* Testing
* Production

Each environment requires:

* Resource Group
* Virtual Network
* Storage Account
* Virtual Machine

Without modules, we would have to write the same Terraform code repeatedly.

```text
Development
 ├── Resource Group
 ├── Storage Account
 ├── Virtual Network
 └── Virtual Machine

Testing
 ├── Resource Group
 ├── Storage Account
 ├── Virtual Network
 └── Virtual Machine

Production
 ├── Resource Group
 ├── Storage Account
 ├── Virtual Network
 └── Virtual Machine
```

This creates duplicate code, increases maintenance effort, and introduces inconsistencies.

Terraform Modules solve these problems.

---

# Real-Life Analogy

Imagine you're building houses.

Every house requires:

* Kitchen
* Washroom
* Bedroom
* Hall

Instead of designing every house from scratch, you create one **house blueprint**.

Whenever a new house is required, you simply reuse the same blueprint.

```text
House Blueprint
       │
 ┌─────┼─────┐
 │     │     │
House A House B House C
```

Similarly,

One Terraform Module → Multiple Infrastructure Deployments.

---

# Infrastructure Before Modules

```text
terraform_project/

├── provider.tf
├── resource_group.tf
├── storage_account.tf
├── vm.tf
├── vnet.tf
└── terraform.tfstate
```

All resources are stored in a single folder.

This approach is acceptable for small projects but becomes difficult to manage in enterprise environments.

---

# Problems Without Modules

## 1. Duplicate Code

The same resource definitions are copied across projects.

---

## 2. Difficult Maintenance

Suppose the VM size needs to change.

Without modules, the change must be made separately in:

* Development
* Testing
* Production

---

## 3. Inconsistent Infrastructure

Different engineers may configure resources differently.

Example:

```text
Development → Standard_B2s

Testing → Standard_D2s
```

Infrastructure becomes inconsistent.

---

## 4. Poor Project Structure

All resources remain mixed together.

```text
provider.tf
resource_group.tf
storage.tf
vm.tf
vnet.tf
outputs.tf
variables.tf
```

Understanding and maintaining the project becomes difficult.

---

## 5. No Reusability

Developers repeatedly copy and paste Terraform code.

---

# Terraform Workflow

## terraform init

When we execute:

```bash
terraform init
```

Terraform performs the following steps:

### Step 1

Scans all `.tf` files.

```text
provider.tf
main.tf
storage.tf
vm.tf
```

---

### Step 2

Reads the Terraform block.

```terraform
terraform {

}
```

---

### Step 3

Downloads required provider plugins.

Example:

* AzureRM Provider

---

### Step 4

Initializes the backend.

If a remote backend exists, Terraform connects to the remote state file.

---

# terraform apply Workflow

When we execute:

```bash
terraform apply
```

Terraform performs the following steps:

### Step 1

Scans all Terraform files.

### Step 2

Reads all resource blocks.

### Step 3

Creates the dependency graph.

### Step 4

Acquires a state lock.

Only one user can update the state file at a time.

### Step 5

Refreshes the current infrastructure state.

### Step 6

Compares:

* Desired State
* Current State

### Step 7

Creates an execution plan.

Example:

```text
+ Resource Group

+ Storage Account

+ Virtual Machine
```

### Step 8

Calls Azure Resource Manager (ARM) APIs.

### Step 9

Creates Azure resources.

### Step 10

Updates the Terraform state file.

```text
terraform.tfstate
```

---

# Terraform Modules Architecture

A typical enterprise project creates one module for each resource.

```text
Modules

├── Resource Group
├── Storage Account
├── Virtual Network
└── Virtual Machine
```

Each module performs one specific task.

---

# Parent Module

The Parent Module is the main Terraform project.

It calls multiple child modules.

```text
Parent Module

├── Resource Group Module
├── Storage Module
├── Network Module
└── Virtual Machine Module
```

---

# Child Module

A Child Module is called by another module.

```text
modules/

├── resource_group/
├── storage/
├── network/
└── virtual_machine/
```

Each child module contains its own:

* main.tf
* variables.tf
* outputs.tf

---

# Recommended Project Structure

```text
terraform_project/

├── provider.tf
├── backend.tf
├── versions.tf
├── variables.tf
├── outputs.tf
├── main.tf
├── terraform.tfstate
│
└── modules/
    │
    ├── resource_group/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── storage/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── network/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    └── virtual_machine/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# Calling Modules

## Resource Group Module

```terraform
module "resource_group" {
  source = "./modules/resource_group"
}
```

---

## Storage Module

```terraform
module "storage" {
  source = "./modules/storage"
}
```

---

## Network Module

```terraform
module "network" {
  source = "./modules/network"
}
```

---

## Virtual Machine Module

```terraform
module "virtual_machine" {
  source = "./modules/virtual_machine"
}
```

Terraform automatically reads all files inside the specified module directory.

---

# Reusability

A single module can be reused multiple times.

```text
Module

      │
      ▼

Development

      │
      ▼

Testing

      │
      ▼

Production
```

Only variable values change.

The code remains the same.

---

# Generic Code

A good module should never contain hardcoded values.

❌ Bad Example

```terraform
name = "cloudtechhacks-rg"
```

✅ Good Example

```terraform
name = var.resource_group_name
```

Generic code makes the module reusable for different environments.

---

# Advantages of Terraform Modules

* Reusable code
* Cleaner project structure
* Less duplicate code
* Easier maintenance
* Easier testing
* Faster development
* Better collaboration
* Standardized infrastructure
* Scalable architecture
* Reduced configuration errors

---

# Problems Without Modules

Suppose every project contains its own provider configuration.

```text
Project-A

provider.tf

terraform init
```

```text
Project-B

provider.tf

terraform init
```

```text
Project-C

provider.tf

terraform init
```

Problems:

* Provider plugins downloaded repeatedly
* Multiple `.terraform` directories
* Separate initialization required
* Multiple state files
* Higher storage usage
* Difficult maintenance

---

# Best Practices

* Create one module for one resource type.
* Avoid hardcoded values.
* Use variables and outputs.
* Keep provider configuration in the parent module.
* Store reusable modules inside a `modules/` directory.
* Use version control (Git).
* Keep modules small and focused.
* Follow a standard folder structure.

---

# Interview Questions

## Q1. What is a Terraform Module?

A Terraform Module is a reusable collection of Terraform configuration files used to provision infrastructure resources.

---

## Q2. What is the difference between a Parent Module and a Child Module?

| Parent Module          | Child Module               |
| ---------------------- | -------------------------- |
| Main Terraform project | Reusable module            |
| Calls other modules    | Gets called by parent      |
| Controls deployment    | Creates specific resources |

---

## Q3. What are the advantages of Terraform Modules?

* Code reusability
* Easy maintenance
* Reduced duplication
* Better collaboration
* Cleaner project structure
* Standardized infrastructure
* Scalability

---

## Q4. Can one module call another module?

Yes.

Terraform supports nested modules, allowing one module to call another module.

---

## Q5. Where can Terraform Modules be stored?

Terraform Modules can be stored in:

* Local filesystem
* GitHub
* GitLab
* Azure Repos
* Terraform Registry
* Private Terraform Registry
* Amazon S3
* Any supported version control system

---

# Summary

* A Terraform Module is a reusable collection of Terraform code.
* Modules eliminate duplicate code.
* Parent Modules call Child Modules.
* Child Modules create specific resources.
* Modules improve maintainability, scalability, and consistency.
* Enterprise projects commonly create separate modules for Resource Groups, VNets, Storage Accounts, and Virtual Machines, then reuse them across Development, Testing, and Production environments.
