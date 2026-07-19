# 📘 Terraform `count` + `list` (Complete Notes)

## Class 38 – Terraform Count + List

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is `count`
- Why we use `count`
- What is a `list`
- How to create multiple Azure resources using `count`
- How `count.index` works
- Difference between `count` and `for_each`
- Real-time examples
- Interview Questions
- Best Practices

---

# What is `count`?

`count` is a Terraform **Meta-Argument** that allows us to create multiple copies of the same resource.

Instead of writing the same resource block multiple times, Terraform automatically creates multiple resources.

## Without `count`

```hcl
resource "azurerm_resource_group" "rg1" {
  name     = "dev-rg"
  location = "Central India"
}

resource "azurerm_resource_group" "rg2" {
  name     = "test-rg"
  location = "Central India"
}

resource "azurerm_resource_group" "rg3" {
  name     = "prod-rg"
  location = "Central India"
}
```

❌ Too much repeated code.

---

## Using `count`

```hcl
resource "azurerm_resource_group" "rg" {
  count    = 3
  name     = "demo-rg-${count.index}"
  location = "Central India"
}
```

Terraform creates:

- demo-rg-0
- demo-rg-1
- demo-rg-2

---

# What is `count.index`?

`count.index` returns the current resource index.

It always starts from **0**.

Example:

```hcl
count = 4
```

Indexes:

- 0
- 1
- 2
- 3

---

# What is a List?

A **List** is an ordered collection of values.

```hcl
[
  "Dev",
  "Test",
  "QA",
  "Prod"
]
```

| Index | Value |
|------:|-------|
| 0 | Dev |
| 1 | Test |
| 2 | QA |
| 3 | Prod |

---

# List Variable

```hcl
variable "rg_names" {
  type = list(string)

  default = [
    "Dev-RG",
    "Test-RG",
    "QA-RG",
    "Prod-RG"
  ]
}
```

---

# Access List Values

```hcl
var.rg_names[0]
```

Output:

```text
Dev-RG
```

```hcl
var.rg_names[2]
```

Output:

```text
QA-RG
```

---

# `count` + `list`

```hcl
variable "rg_names" {
  type = list(string)

  default = [
    "dev-rg",
    "test-rg",
    "qa-rg",
    "prod-rg"
  ]
}

resource "azurerm_resource_group" "rg" {
  count    = length(var.rg_names)
  name     = var.rg_names[count.index]
  location = "Central India"
}
```

Terraform iterations:

| count.index | Resource |
|------------:|----------|
| 0 | dev-rg |
| 1 | test-rg |
| 2 | qa-rg |
| 3 | prod-rg |

---

# Dynamic Location Example

```hcl
variable "rg_names" {
  default = [
    "dev-rg",
    "test-rg",
    "qa-rg",
    "prod-rg"
  ]
}

variable "locations" {
  default = [
    "Central India",
    "East US",
    "West Europe",
    "Japan East"
  ]
}

resource "azurerm_resource_group" "rg" {
  count    = length(var.rg_names)
  name     = var.rg_names[count.index]
  location = var.locations[count.index]
}
```

---

# Count with Boolean

```hcl
variable "create_rg" {
  default = true
}

resource "azurerm_resource_group" "rg" {
  count    = var.create_rg ? 1 : 0
  name     = "demo-rg"
  location = "Central India"
}
```

---

# Count with Virtual Machines

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  count    = 3
  name     = "linux-vm-${count.index}"
  location = "Central India"
}
```

---

# Count Limitations

`count` is **index-based**.

If you change the order of a list after deployment, Terraform may destroy and recreate resources because the indexes change.

---

# Count vs `for_each`

| Count | for_each |
|--------|----------|
| Uses index | Uses key |
| Best for identical resources | Best for unique resources |
| Supports lists | Supports maps and sets |
| Reordering may recreate resources | Stable even after reordering |

---

# Best Practices

- ✅ Use `count` for identical resources.
- ✅ Use `for_each` for uniquely identified resources.
- ✅ Use `length()` for dynamic resource creation.
- ✅ Keep related lists synchronized.
- ✅ Avoid changing list order after deployment.

---

# Common Mistakes

### Invalid Index

```hcl
count = 5
```

If the list contains only **3** elements:

```text
Error: Invalid index
```

---

# Interview Questions

### 1. What is `count`?

A Terraform Meta-Argument used to create multiple instances of a resource.

### 2. What does `count.index` return?

The current iteration index, starting from **0**.

### 3. Can `count` work with a list?

Yes.

```hcl
count = length(var.rg_names)
name  = var.rg_names[count.index]
```

### 4. What happens if the list order changes?

Terraform may destroy and recreate resources because `count` depends on indexes.

### 5. When should you use `count`?

- Identical resources
- Sequential naming
- Optional resource creation

### 6. When should you avoid `count`?

When resources have unique configurations or when list order may change.

---

# Summary

- `count` creates multiple resource instances.
- `count.index` starts from **0**.
- Lists store ordered values.
- `length()` determines the number of resources.
- `count` + `list` enables dynamic infrastructure creation.
- Prefer `for_each` for stable, uniquely identified resources.
