# Class 42 – Terraform `for_each` + Map

## 1. Introduction

Terraform mein jab hume same type ke multiple resources create karne hote hain, tab har resource ko manually define karna practical nahi hota.

For example, agar hume 5 Azure Resource Groups create karne hain:

- `rg1`
- `rg2`
- `rg3`
- `rg4`
- `rg5`

Toh 5 alag resource blocks likhne ke bajay hum `for_each` ka use kar sakte hain.

### Basic idea

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

Yahaan Terraform input collection ke har element ke liye ek resource instance create karega.

---

# 2. Why do we use `for_each`?

Without `for_each`, hume resources manually create karne pad sakte hain:

```hcl
resource "azurerm_resource_group" "rg1" {
  name     = "rg1"
  location = "Central India"
}

resource "azurerm_resource_group" "rg2" {
  name     = "rg2"
  location = "Central US"
}

resource "azurerm_resource_group" "rg3" {
  name     = "rg3"
  location = "Central Canada"
}
```

Agar resource groups 50 ya 100 ho jaayein, configuration unnecessarily large ho jayegi.

`for_each` ke saath:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

Ab sirf input variable change karke multiple resources create kiye ja sakte hain.

---

# 3. `for_each` ke Important Input Types

Terraform `for_each` primarily in do collection types ke saath use kiya jata hai:

1. Set of Strings
2. Map

Class diagram mein bhi `for_each` ke neeche ye dono input types show kiye gaye hain.

---

# 4. `for_each` with Set of String

Example:

```hcl
variable "rg_names" {
  type = set(string)
}
```

Value:

```hcl
rg_names = [
  "rg1",
  "rg2",
  "rg3"
]
```

Better explicitly as a set:

```hcl
rg_names = toset([
  "rg1",
  "rg2",
  "rg3"
])
```

## Resource

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rg_names

  name     = each.value
  location = "Central India"
}
```

Yahaan har element ke liye ek Resource Group create hoga.

### Input

```text
rg1
rg2
rg3
```

### Terraform approximately create karega

```text
azurerm_resource_group.rgs["rg1"]
azurerm_resource_group.rgs["rg2"]
azurerm_resource_group.rgs["rg3"]
```

Iska matlab:

> Set mein jitne elements honge, `for_each` utni resource instances create karega.

---

# 5. `each.key` and `each.value`

`for_each` use karte waqt Terraform do important values provide karta hai:

```hcl
each.key
each.value
```

### Map ke case mein

```text
each.key   → Map ki key
each.value → Map ki value
```

Example:

```hcl
rgs = {
  "Central India" = "rg1"
  "Central US"    = "rg2"
  "Central Canada" = "rg3"
}
```

First iteration:

```text
each.key   = "Central India"
each.value = "rg1"
```

Second iteration:

```text
each.key   = "Central US"
each.value = "rg2"
```

Third iteration:

```text
each.key   = "Central Canada"
each.value = "rg3"
```

---

# 6. `for_each` with Map

Map ka structure:

```hcl
{
  key = value
}
```

Example:

```hcl
variable "rgs" {
  type = map(string)
}
```

`terraform.tfvars`:

```hcl
rgs = {
  "rg1" = "Central India"
  "rg2" = "Central US"
  "rg3" = "Central Canada"
  "rg4" = "South India"
  "rg5" = "West India"
}
```

Yahaan:

```text
Key   → rg1
Value → Central India
```

```text
Key   → rg2
Value → Central US
```

---

# 7. Map ka Basic Concept

Class diagram mein specifically explain kiya gaya hai ki Map ka structure:

```hcl
key = value
```

hota hai.

Example:

```hcl
{
  "rg1" = "Central India"
  "rg2" = "Central US"
  "rg3" = "Central Canada"
}
```

Yahaan keys unique honi chahiye.

### Important

Map mein key generally string hoti hai, jabki value declared map type ke according hoti hai.

Example:

```hcl
map(string)
```

means:

```text
key   → string
value → string
```

---

# 8. Simple Map Example

```hcl
variable "rgs" {
  type = map(string)
}
```

Input:

```hcl
rgs = {
  rg1 = "Central India"
  rg2 = "Central US"
  rg3 = "Central Canada"
}
```

Resource:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

Terraform instances:

```text
azurerm_resource_group.rgs["rg1"]
azurerm_resource_group.rgs["rg2"]
azurerm_resource_group.rgs["rg3"]
```

---

# 9. How the Loop Works

Suppose:

```hcl
rgs = {
  rg1 = "Central India"
  rg2 = "Central US"
  rg3 = "Central Canada"
}
```

Terraform internally conceptually process karega:

### Iteration 1

```text
each.key   = rg1
each.value = Central India
```

Resource:

```hcl
name     = "rg1"
location = "Central India"
```

### Iteration 2

```text
each.key   = rg2
each.value = Central US
```

Resource:

```hcl
name     = "rg2"
location = "Central US"
```

### Iteration 3

```text
each.key   = rg3
each.value = Central Canada
```

Resource:

```hcl
name     = "rg3"
location = "Central Canada"
```

Therefore:

> Map mein jitni keys hongi, utni resource instances create hongi.

---

# 10. Important Example from Class

Class diagram mein example diya gaya hai:

```hcl
rgs = {
  "East US"       = "rg1"
  "West Europe"   = "rg2"
  "Southeast Asia" = "rg3"
}
```

Resource:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.value
  location = each.key
}
```

Notice carefully:

```hcl
name     = each.value
location = each.key
```

### First iteration

```text
each.key   = East US
each.value = rg1
```

Result:

```text
Name     = rg1
Location = East US
```

### Second iteration

```text
each.key   = West Europe
each.value = rg2
```

Result:

```text
Name     = rg2
Location = West Europe
```

### Third iteration

```text
each.key   = Southeast Asia
each.value = rg3
```

Result:

```text
Name     = rg3
Location = Southeast Asia
```

---

# 11. Resource Address with `for_each`

Normal resource:

```hcl
azurerm_resource_group.rg
```

`for_each` ke saath resource multiple instances mein convert ho jata hai.

Example:

```hcl
azurerm_resource_group.rgs["rg1"]
azurerm_resource_group.rgs["rg2"]
azurerm_resource_group.rgs["rg3"]
```

Ye Terraform state mein bhi individually track hote hain.

This is one major difference from creating only a single resource.

---

# 12. `for_each` vs Normal Resource

### Without `for_each`

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "cloud-rg"
  location = "Central India"
}
```

Only one resource.

### With `for_each`

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

Multiple resources.

---

# 13. `for_each` vs `count`

Both multiple resources create kar sakte hain, lekin dono ka behavior different hai.

## `count`

Usually numeric index use karta hai:

```hcl
resource "azurerm_resource_group" "rgs" {
  count = 3

  name     = "rg-${count.index}"
  location = "Central India"
}
```

Addresses:

```text
azurerm_resource_group.rgs[0]
azurerm_resource_group.rgs[1]
azurerm_resource_group.rgs[2]
```

## `for_each`

Meaningful keys use kar sakta hai:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

Addresses:

```text
azurerm_resource_group.rgs["rg1"]
azurerm_resource_group.rgs["rg2"]
azurerm_resource_group.rgs["rg3"]
```

### Practical preference

Agar resources ko meaningful names/keys se identify karna hai, `for_each` generally easier to manage hota hai.

---

# 14. Map with Different Information

Real projects mein sirf Resource Group ka name aur location enough nahi hota.

Hume additional information bhi chahiye ho sakti hai:

```text
rg_name
location
managed_by
environment
owner
tags
```

Is situation mein simple:

```hcl
map(string)
```

sufficient nahi hota.

Yahaan hum **Nested Map / Map of Objects** ka concept use kar sakte hain.

---

# 15. Nested Map

Class diagram ke bottom section mein Nested Map ka example diya gaya hai.

Conceptually:

```hcl
rg_names = {
  rg1 = {
    rg_name    = "cloud-rg1"
    location   = "Central India"
    managed_by = "terraform"
  }
}
```

Yahaan outer map ke andar ek aur map/object structure hai.

Structure:

```text
Outer Map
│
├── rg1
│     ├── rg_name
│     ├── location
│     └── managed_by
│
├── rg2
│     ├── rg_name
│     ├── location
│     └── managed_by
│
└── rg3
      ├── rg_name
      ├── location
      └── managed_by
```

---

# 16. Why Nested Map?

Suppose hume 3 Resource Groups create karne hain:

```text
rg1 → Central India → Terraform
rg2 → Central US → Terraform
rg3 → West Europe → Terraform
```

Simple map:

```hcl
{
  rg1 = "Central India"
  rg2 = "Central US"
  rg3 = "West Europe"
}
```

sirf do pieces of information manage kar raha hai.

Nested structure:

```hcl
{
  rg1 = {
    rg_name    = "cloud-rg1"
    location   = "Central India"
    managed_by = "terraform"
  }

  rg2 = {
    rg_name    = "cloud-rg2"
    location   = "Central US"
    managed_by = "terraform"
  }
}
```

multiple attributes ko ek resource ke saath associate kar sakta hai.

---

# 17. Nested Map – Conceptual Variable

Example:

```hcl
variable "rg_names" {
  type = map(object({
    rg_name    = string
    location   = string
    managed_by = string
  }))
}
```

Input:

```hcl
rg_names = {
  rg1 = {
    rg_name    = "cloud-rg1"
    location   = "Central India"
    managed_by = "terraform"
  }

  rg2 = {
    rg_name    = "cloud-rg2"
    location   = "Central US"
    managed_by = "terraform"
  }

  rg3 = {
    rg_name    = "cloud-rg3"
    location   = "West Europe"
    managed_by = "terraform"
  }
}
```

---

# 18. Accessing Nested Map Values

Agar:

```hcl
for_each = var.rg_names
```

hai, toh:

```hcl
each.key
```

outer map ki key dega.

Example:

```text
each.key = rg1
```

Aur:

```hcl
each.value
```

poora inner object/map dega.

Example:

```text
each.value = {
  rg_name    = "cloud-rg1"
  location   = "Central India"
  managed_by = "terraform"
}
```

Specific attribute:

```hcl
each.value.rg_name
```

```hcl
each.value.location
```

```hcl
each.value.managed_by
```

---

# 19. Nested Map with Azure Resource Group

Example:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rg_names

  name     = each.value.rg_name
  location = each.value.location
}
```

Agar tags bhi use karne hain:

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rg_names

  name     = each.value.rg_name
  location = each.value.location

  tags = {
    ManagedBy = each.value.managed_by
  }
}
```

This approach real-world infrastructure ke liye much more useful hai.

---

# 20. `for_each` + Nested Map – Mental Model

Always remember:

```text
for_each
   │
   └── outer map
          │
          ├── each.key
          │
          └── each.value
                 │
                 ├── attribute 1
                 ├── attribute 2
                 └── attribute 3
```

Example:

```text
var.rg_names
     │
     ├── rg1
     │    ├── rg_name
     │    ├── location
     │    └── managed_by
     │
     └── rg2
          ├── rg_name
          ├── location
          └── managed_by
```

---

# 21. Important Rule – Keys Must Be Unique

Map mein keys unique honi chahiye.

Correct:

```hcl
{
  rg1 = "Central India"
  rg2 = "Central US"
}
```

Incorrect concept:

```hcl
{
  rg1 = "Central India"
  rg1 = "Central US"
}
```

Same key ko meaningful way mein duplicate nahi kiya ja sakta.

Isliye `for_each` ke saath stable, meaningful keys rakhna important hai.

---

# 22. Practical Azure Use Cases

Class diagram mein multiple Azure services ko child modules ke examples ke roop mein mention kiya gaya hai:

- Resource Group
- VNet
- Subnet
- VNet Peering
- VPN Gateway
- Key Vault
- Bastion
- VM
- VMSS
- Azure Function
- Logic Apps
- AKS
- ACI
- Load Balancer
- Front Door
- NSG
- ASG
- Firewall
- Backup
- Log Analytics

In sab resources ke multiple instances create karne ke liye `for_each` ka pattern bahut useful ho sakta hai.

Example:

```text
Multiple Resource Groups
        ↓
Multiple VNets
        ↓
Multiple Subnets
        ↓
Multiple NSGs
        ↓
Multiple VMs
```

Terraform mein input-driven infrastructure design karna easy ho jata hai.

---

# 23. `for_each` in Child Modules

Class diagram ke bottom-right mein **Child Modules** aur **Parent Module** ka relationship bhi show kiya gaya hai.

Same concept resources ke saath modules par bhi apply kiya ja sakta hai.

Example:

```hcl
module "resource_group" {
  for_each = var.resource_groups

  source = "./modules/resource-group"

  name     = each.value.name
  location = each.value.location
}
```

Agar input mein:

```hcl
resource_groups = {
  rg1 = {
    name     = "cloud-rg1"
    location = "Central India"
  }

  rg2 = {
    name     = "cloud-rg2"
    location = "Central US"
  }
}
```

toh module ke multiple instances create honge.

Conceptually:

```text
Parent Module
     │
     ├── Child Module → rg1
     │
     ├── Child Module → rg2
     │
     └── Child Module → rg3
```

---

# 24. Parent Module vs Child Module

### Parent Module

Main Terraform configuration jahan se modules call kiye ja rahe hain.

Example:

```hcl
module "resource_group" {
  source = "./modules/resource-group"

  ...
}
```

### Child Module

Reusable Terraform code:

```text
modules/
└── resource-group/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

Parent module input provide karta hai aur child module actual resource configuration contain karta hai.

---

# 25. Recommended Project Structure

Real project mein:

```text
terraform-project/
│
├── main.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
│
└── modules/
    │
    ├── resource-group/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── vnet/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── vm/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    └── key-vault/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# 26. Real-World Architecture Example

Suppose company ke paas multiple environments hain:

```text
Development
QA
Production
```

Har environment ke liye different Resource Groups chahiye.

Input:

```hcl
resource_groups = {
  dev = {
    name     = "dev-rg"
    location = "Central India"
  }

  qa = {
    name     = "qa-rg"
    location = "Central India"
  }

  prod = {
    name     = "prod-rg"
    location = "West Europe"
  }
}
```

Module:

```hcl
module "resource_group" {
  for_each = var.resource_groups

  source = "./modules/resource-group"

  name     = each.value.name
  location = each.value.location
}
```

Is approach se ek hi module ko multiple environments ke liye reuse kiya ja sakta hai.

---

# 27. Common Mistakes

## Mistake 1 – `each.key` aur `each.value` ko confuse karna

Map:

```hcl
{
  rg1 = "Central India"
}
```

Correct:

```text
each.key   = rg1
each.value = Central India
```

---

## Mistake 2 – List directly use karna

`for_each` ke liye normal list ko directly use karne ke bajay set/map use karna preferred hai.

Example:

```hcl
for_each = toset(var.rg_names)
```

---

## Mistake 3 – Wrong attribute access

Nested map mein:

```hcl
each.value
```

poora object ho sakta hai.

Specific value ke liye:

```hcl
each.value.location
```

use karna padega.

---

## Mistake 4 – Duplicate keys

Map keys unique honi chahiye.

---

# 28. `for_each` – Quick Revision

Remember this formula:

```text
for_each
   ↓
Collection
   ↓
Map / Set
   ↓
Multiple iterations
   ↓
each.key
each.value
   ↓
Multiple resource instances
```

For Map:

```text
each.key   → key
each.value → value
```

For Nested Map/Object:

```text
each.key          → outer key
each.value        → object
each.value.name   → object attribute
each.value.region → object attribute
```

---

# 29. Interview Questions

### Q1. What is `for_each` in Terraform?

`for_each` is a Terraform meta-argument used to create multiple resource or module instances based on a map or set of strings.

### Q2. What are `each.key` and `each.value`?

For a map:

```text
each.key   → current map key
each.value → current map value
```

### Q3. What types can commonly be used with `for_each`?

```text
Set of strings
Map
```

### Q4. How is `for_each` different from `count`?

`count` uses numeric indexes, whereas `for_each` uses keys from a map or values from a set.

### Q5. What happens if a map has 5 keys?

Terraform creates 5 instances of the resource/module.

### Q6. Why is `for_each` useful in production?

Because it allows reusable, scalable, data-driven infrastructure without duplicating resource blocks.

### Q7. What is a nested map/object?

It is a structure where an outer map contains values that themselves contain multiple attributes.

Example:

```hcl
{
  rg1 = {
    name     = "cloud-rg1"
    location = "Central India"
  }
}
```

---

# 30. Final Class Summary

Class 42 ka main focus:

```text
Terraform for_each
       ↓
Set of String
       ↓
Map
       ↓
each.key
       ↓
each.value
       ↓
Nested Map / Object
       ↓
Child Modules
       ↓
Reusable Azure Infrastructure
```

### Most Important Syntax

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.key
  location = each.value
}
```

### Nested Object Syntax

```hcl
resource "azurerm_resource_group" "rgs" {
  for_each = var.rgs

  name     = each.value.rg_name
  location = each.value.location
}
```

### Golden Rule

> **Map ke case mein `each.key` key ko represent karta hai aur `each.value` value ko represent karta hai. Nested object/map ke case mein `each.value` ke andar further attributes access kiye ja sakte hain.**

**Class 42 ka practical objective:**  
Ek reusable Terraform configuration ke through multiple Azure resources/modules ko dynamically create karna, without writing the same resource block repeatedly.