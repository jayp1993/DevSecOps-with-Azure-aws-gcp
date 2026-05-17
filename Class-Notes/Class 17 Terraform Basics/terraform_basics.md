# Terraform Basics – Detailed Notes (Class 17)

## What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool developed by [HashiCorp](https://www.hashicorp.com/?utm_source=chatgpt.com).
It helps engineers create, manage, and automate cloud infrastructure using code instead of manual operations.

Terraform follows a **Declarative Approach**, where we define the desired infrastructure state, and Terraform automatically creates and manages the resources.

Example:

* Resource Groups
* Virtual Machines
* Storage Accounts
* Networks
* Databases
* Load Balancers



---

# Why Do We Need Automation?

In cloud environments, infrastructure can become very large and complex.

Example:

* Managing 5 servers manually is possible.
* Managing 500 servers manually becomes difficult.

Manual infrastructure management causes:

* More time consumption
* Human errors
* Poor consistency
* Difficult maintenance
* No tracking of changes

That is why organizations move toward **Infrastructure Automation**.



---

# Infrastructure Management Approaches

# 1. Manual Management

Infrastructure is created manually using tools such as:

* Azure Portal
* Azure CLI
* Azure PowerShell
* Azure REST API

## Problems with Manual Management

* Time-consuming
* Difficult to manage large environments
* High chances of configuration mistakes
* No proper state tracking
* Difficult readability and maintenance
* Reusability is limited

Example:
If one engineer creates resources manually, another engineer may not know:

* What was created
* Which settings were used
* What changes were made



---

# 2. Automation

Automation means infrastructure creation using scripts or code.

Automation is mainly divided into two types:

## A. Imperative Approach

In the Imperative approach, we write step-by-step instructions.

Example:

```text
1. Create Resource Group
2. Create Virtual Network
3. Create Subnet
4. Create VM
```

### Tools Used

* Azure CLI
* Azure PowerShell
* Azure REST API

### Limitations

* Difficult for complex infrastructure
* No strong state management
* Hard to maintain large codebases
* Reusability is limited



---

## B. Declarative Approach

In the Declarative approach, we only define:

* What infrastructure we want

We do not define every step manually.

Example:

```text
I need:
- 2 Virtual Machines
- 1 Resource Group
- 1 Storage Account
- 1 Virtual Network
```

Terraform automatically handles:

* Resource creation order
* Dependencies
* Configuration logic

### Benefits

* Easier management
* Better scalability
* Reusable code
* Faster deployment
* Better consistency



---

# Native Tools vs Open Source Tools

# Native Tools

Native tools are cloud-provider-specific tools.

## Azure Native Tools

* ARM Templates
* Bicep

## AWS Native Tool

* CloudFormation

### Limitation of Native Tools

Native tools usually support only their own cloud platform.

Example:

* ARM Templates work only in Azure
* CloudFormation works only in AWS



---

# Open Source IaC Tools

# Terraform

Terraform is the most popular open-source Infrastructure as Code tool.

## Key Features

* Multi-cloud support
* State management
* Declarative syntax
* Modular design
* Reusable code
* Infrastructure automation

## Supported Platforms

* Azure
* AWS
* Google Cloud
* VMware
* Kubernetes
* Oracle Cloud

Terraform supports **multi-cloud environments**.



---

# What is State Management in Terraform?

State management is one of Terraform’s most important features.

Terraform stores infrastructure information in a file called:

```text
terraform.tfstate
```

This file keeps track of:

* Created resources
* Configuration details
* Infrastructure changes

## Benefits

* Detects infrastructure drift
* Tracks changes
* Avoids duplicate resource creation
* Supports updates and deletions

Without state management:

* Infrastructure tracking becomes difficult
* Automation becomes unreliable



---

# Terraform Architecture

Terraform uses:

* Frontend configuration files
* Backend state management
* Providers for cloud communication

## Terraform Provider

A provider is a plugin that allows Terraform to communicate with cloud platforms.

Example:

* Azure Provider
* AWS Provider
* Kubernetes Provider

Example:

```hcl
provider "azurerm" {
  features {}
}
```

---

# Terraform Configuration Language

Terraform uses:

* HCL (HashiCorp Configuration Language)

HCL is:

* Human-readable
* Easy to understand
* Easy to maintain

Example:

```hcl
resource "azurerm_resource_group" "rg1" {
  name     = "demo-rg"
  location = "Central India"
}
```

---

# Terraform File Extensions

Terraform files use:

```text
.tf
```

Example:

```text
main.tf
variables.tf
outputs.tf
terraform.tfvars
```

---

# Terraform Workflow

Terraform generally follows this workflow:

## 1. Write Code

Create Terraform configuration files.

## 2. Initialize Terraform

```bash
terraform init
```

Downloads:

* Providers
* Plugins
* Backend initialization

---

## 3. Validate Configuration

```bash
terraform validate
```

Checks syntax and configuration errors.

---

## 4. Format Code

```bash
terraform fmt
```

Formats code properly.

---

## 5. Create Execution Plan

```bash
terraform plan
```

Shows:

* What Terraform will create
* What Terraform will modify
* What Terraform will delete

---

## 6. Deploy Infrastructure

```bash
terraform apply
```

Actually creates the infrastructure.

---

## 7. Destroy Infrastructure

```bash
terraform destroy
```

Deletes created resources.

---

# Prerequisites for Terraform

Before starting Terraform:

## Required Software

* Terraform installed
* VS Code installed
* Azure account
* Azure subscription
* Contributor access



---

# Components Needed for Terraform Coding

## 1. Explorer

Used for:

* Creating files
* Managing folders

## 2. Editor

Used for writing code.

Examples:

* VS Code
* Notepad++

## 3. Terminal

Used for executing Terraform commands.

Commands:

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```



---

# Documentation Search Strategy

Good engineers know how to search documentation effectively.

## Google Search Example

```text
How to create resource group in Azure using Terraform
```

## Official Documentation

### Terraform

[Terraform Registry](https://registry.terraform.io/?utm_source=chatgpt.com)

### Azure

[Microsoft Learn](https://learn.microsoft.com/?utm_source=chatgpt.com)

### AWS

[AWS Documentation](https://docs.aws.amazon.com/?utm_source=chatgpt.com)



---

# Terraform vs ARM Templates

| Feature           | Terraform   | ARM Templates |
| ----------------- | ----------- | ------------- |
| Cloud Support     | Multi-cloud | Azure only    |
| Language          | HCL         | JSON          |
| Readability       | Easy        | Complex       |
| State Management  | Yes         | Limited       |
| Open Source       | Yes         | No            |
| Community Support | Huge        | Limited       |

---

# Advantages of Terraform

## Major Benefits

* Infrastructure automation
* Multi-cloud support
* Reusable modules
* Version control integration
* Easy collaboration
* State management
* Faster deployment
* Consistent environments

---

# Real-World Use Cases

Terraform is commonly used for:

* VM deployment
* Kubernetes clusters
* Networking setup
* Disaster Recovery environments
* CI/CD automation
* Multi-cloud environments
* Infrastructure replication

---

# Important Interview Questions

## What is Infrastructure as Code?

Infrastructure management using code instead of manual operations.

---

## Why is Terraform popular?

Because of:

* Multi-cloud support
* State management
* Easy syntax
* Strong community support

---

## What is the difference between Imperative and Declarative?

* Imperative = Step-by-step instructions
* Declarative = Desired final state

---

## What is Terraform State File?

A file that stores infrastructure information and tracks resource changes.

---

## What language does Terraform use?

HCL (HashiCorp Configuration Language)

---

# Summary

Terraform is a powerful Infrastructure as Code tool used for:

* Cloud automation
* Multi-cloud infrastructure
* Scalable deployments
* State-managed infrastructure provisioning

It simplifies infrastructure management and is one of the most important tools in modern DevOps and Cloud Engineering.

Based on your uploaded Class 17 PDF notes. 
