# Class 35 -- Terraform Variables (Detailed Notes)

> Based on Class 35 PDF (Terraform Variables)

## What are Terraform Variables?

Terraform Variables make Infrastructure as Code reusable, flexible, and
dynamic. Instead of hardcoding values, store them in variables and reuse
the same code across Dev, QA, UAT, and Production.

### Without Variables

``` hcl
resource "azurerm_resource_group" "rg" {
  name     = "jay-rg"
  location = "Central India"
}
```

### With Variables

``` hcl
variable "rg_name" {}
variable "location" {}

resource "azurerm_resource_group" "rg" {
  name     = var.rg_name
  location = var.location
}
```

------------------------------------------------------------------------

# Why Use Variables?

-   Reusable Code
-   Dynamic Infrastructure
-   Easier Maintenance
-   Environment Independent
-   Production Ready
-   Better Security

------------------------------------------------------------------------

# Variable Lifecycle

``` text
Declare
   ↓
Assign
   ↓
Use
```

------------------------------------------------------------------------

# Declaring Variables

``` hcl
variable "rg_name" {
  type        = string
  description = "Resource Group Name"
}
```

------------------------------------------------------------------------

# Primitive Data Types

## 1. String

``` hcl
variable "vm_name" {
  type = string
}
```

``` hcl
vm_name = "LinuxVM01"
```

-   Uses double quotes
-   Stores text

------------------------------------------------------------------------

## 2. Number

``` hcl
variable "disk_size" {
  type = number
}
```

``` hcl
disk_size = 128
```

-   No quotes
-   Supports integers and decimals

------------------------------------------------------------------------

## 3. Boolean

``` hcl
variable "public_ip" {
  type = bool
}
```

``` hcl
public_ip = true
```

Used for enabling/disabling features.

------------------------------------------------------------------------

# Collection Types

## List

``` hcl
variable "fruits" {
  type = list(string)
}
```

``` hcl
fruits = ["Apple","Mango","Banana","Banana"]
```

-   Ordered
-   Duplicate values allowed
-   Index supported

------------------------------------------------------------------------

## Set

``` hcl
variable "fruits" {
  type = set(string)
}
```

-   Duplicate values removed
-   Unordered

------------------------------------------------------------------------

## Map

``` hcl
variable "country_details" {
  type = map(string)
}
```

``` hcl
country_details = {
  India = "Delhi"
  UK     = "London"
}
```

Access:

``` hcl
var.country_details["India"]
```

------------------------------------------------------------------------

# Using Variables

``` hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type

  tags = {
    Environment = var.environment
    Name        = "${var.environment}-web-server"
  }
}
```

------------------------------------------------------------------------

# Assigning Values

## Default

``` hcl
variable "location" {
  type    = string
  default = "Central India"
}
```

## terraform.tfvars

``` hcl
rg_name  = "jay-rg"
location = "Central India"
```

## Command Line

``` bash
terraform apply -var="location=Central India"
```

## Environment Variable

``` bash
export TF_VAR_location="Central India"
```

------------------------------------------------------------------------

# Variables with Modules

## Parent Module

``` hcl
module "resource_group" {
  source      = "../Child_Module"
  rg_name     = "jay-rg"
  rg_location = "Central India"
}
```

## Child Module

``` hcl
variable "rg_name" {}
variable "rg_location" {}
```

------------------------------------------------------------------------

# Best Practices

-   Use meaningful names
-   Always define `type`
-   Add `description`
-   Store values in `terraform.tfvars`
-   Store declarations in `variables.tf`
-   Never hardcode passwords or secrets
-   Use Azure Key Vault or CI/CD secret stores

------------------------------------------------------------------------

# Interview Questions

## What are Terraform Variables?

Variables make Terraform code reusable and configurable.

## Difference between List and Set?

  List                Set
  ------------------- -----------------------
  Ordered             Unordered
  Duplicate Allowed   Duplicate Not Allowed
  Index Supported     No Index

## Primitive Types

-   String
-   Number
-   Boolean

## Collection Types

-   List
-   Set
-   Map

------------------------------------------------------------------------

# Production Folder Structure

``` text
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── provider.tf
├── versions.tf
└── modules/
    └── resource_group/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

------------------------------------------------------------------------

# Summary

-   Variables = Reusable Infrastructure
-   Workflow = Declare → Assign → Use
-   Access variables using `var.<name>`
-   Use typed variables
-   Follow production folder structure
