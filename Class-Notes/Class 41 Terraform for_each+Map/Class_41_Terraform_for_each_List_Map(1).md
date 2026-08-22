# Class 41: Terraform `for_each` + List + Map

## 1. Terraform Variables – Quick Revision

Terraform variables are used to provide values dynamically instead of hard-coding them.

```hcl
variable "rg_name" {
  type = string
}
```

Example in `terraform.tfvars`:

```hcl
rg_name = "angelone-rg"
```

CLI:

```bash
terraform plan -var="rg_name=angelone-rg"
```

Terraform automatically reads variable values from:

- `terraform.tfvars`
- `*.auto.tfvars`

---

## 2. Without Loop – Single Resource

If we need to create only one Resource Group:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = var.rg_name
  location = "Central India"
}
```

This creates one Resource Group.

---

## 3. Without Loop + List

Suppose we have multiple Resource Group names:

```hcl
rg_names = [
  "angelone-rg1",
  "angelone-rg2",
  "angelone-rg3",
  "angelone-rg4",
  "angelone-rg5"
]
```

A list element can be accessed using its index:

```hcl
var.rg_names[0]
```

Result:

```text
angelone-rg1
```

Similarly:

```hcl
var.rg_names[1]  # angelone-rg2
var.rg_names[2]  # angelone-rg3
```

### Problem

For many resources, manually using indexes is not practical.

This is where Terraform loops such as `count` and `for_each` become useful.

---

# 4. `count` + List

`count` can be used to create multiple instances of a resource.

### Variable

```hcl
variable "rg_names" {
  type = list(string)
}
```

### Input

```hcl
rg_names = [
  "angelone-rg1",
  "angelone-rg2",
  "angelone-rg3",
  "angelone-rg4",
  "angelone-rg5"
]
```

### Resource

```hcl
resource "azurerm_resource_group" "rg" {
  count    = length(var.rg_names)
  name     = var.rg_names[count.index]
  location = "Central India"
}
```

### How `count.index` works

`count.index` provides numeric indexes:

```text
0
1
2
3
4
```

Conceptually:

```text
count.index = 0 → angelone-rg1
count.index = 1 → angelone-rg2
count.index = 2 → angelone-rg3
count.index = 3 → angelone-rg4
count.index = 4 → angelone-rg5
```

---

# 5. Problem with `count`

`count` identifies resources using indexes.

Suppose the original list is:

```hcl
rg_names = [
  "rg1",
  "rg2",
  "rg3",
  "rg4"
]
```

Terraform addresses them approximately as:

```text
rg[0] → rg1
rg[1] → rg2
rg[2] → rg3
rg[3] → rg4
```

Now suppose `rg2` is removed:

```hcl
rg_names = [
  "rg1",
  "rg3",
  "rg4"
]
```

The indexes shift:

```text
rg[0] → rg1
rg[1] → rg3
rg[2] → rg4
```

Because resource identity is index-based, this can result in unwanted resource changes or replacements.

### Important Rule

> `count` identifies resource instances using numeric indexes.

---

# 6. `for_each` + List

`for_each` is useful when resources should have a stable identity based on their elements or keys.

A list is commonly converted to a set before using it with `for_each`:

```hcl
for_each = toset(var.rg_names)
```

### Example

```hcl
variable "rg_names" {
  type = list(string)
}
```

Input:

```hcl
rg_names = [
  "angelone-rg1",
  "angelone-rg2",
  "angelone-rg3",
  "angelone-rg4",
  "angelone-rg5"
]
```

Resource:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = toset(var.rg_names)

  name     = each.value
  location = "Central India"
}
```

---

# 7. `each.key` and `each.value`

`for_each` provides two important values:

```text
each.key
each.value
```

When using a set:

```hcl
for_each = toset(var.rg_names)
```

the element can be accessed with:

```hcl
each.value
```

Example:

```text
each.value = angelone-rg1
each.value = angelone-rg2
each.value = angelone-rg3
```

So:

```hcl
name = each.value
```

assigns the current Resource Group name.

---

# 8. `count` vs `for_each`

| Feature | `count` | `for_each` |
|---|---|---|
| Identity | Numeric index | Key / element |
| Common input | List | Set / Map |
| Access | `count.index` | `each.key`, `each.value` |
| Example | `rg[0]` | `rg["angelone-rg1"]` |
| Deletion handling | Index shift can cause problems | More stable identity |
| Best suited for | Simple repetition | Named / independently managed resources |

### Easy way to remember

> **`count` → index based**

> **`for_each` → element/key based**

---

# 9. `for_each` + Map

A list is useful when we only need a collection of values.

But suppose we need a meaningful key along with each value:

```text
key → value
```

Then a map is more suitable.

Example:

```hcl
rg_names = {
  rg1 = "angelone-rg1"
  rg2 = "angelone-rg2"
  rg3 = "angelone-rg3"
}
```

Conceptually:

```text
rg1 → angelone-rg1
rg2 → angelone-rg2
rg3 → angelone-rg3
```

---

# 10. Map Variable

Define the variable:

```hcl
variable "rg_names" {
  type = map(string)
}
```

Input:

```hcl
rg_names = {
  rg1 = "angelone-rg1"
  rg2 = "angelone-rg2"
  rg3 = "angelone-rg3"
}
```

Resource:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = var.rg_names

  name     = each.value
  location = "Central India"
}
```

Here:

```text
each.key
```

returns:

```text
rg1
rg2
rg3
```

And:

```text
each.value
```

returns:

```text
angelone-rg1
angelone-rg2
angelone-rg3
```

---

# 11. Real-World Use of `for_each` + Map

Maps allow us to give resources meaningful identities.

Example:

```hcl
rg_names = {
  production  = "prod-rg"
  development = "dev-rg"
  testing     = "test-rg"
}
```

Resource:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = var.rg_names

  name     = each.value
  location = "Central India"
}
```

Conceptually Terraform addresses the resources as:

```text
rg["production"]
rg["development"]
rg["testing"]
```

This is easier to understand than:

```text
rg[0]
rg[1]
rg[2]
```

---

# 12. `toset()` – Why Do We Use It?

Suppose the input is a list:

```hcl
rg_names = [
  "rg1",
  "rg2",
  "rg3"
]
```

We can convert it to a set:

```hcl
for_each = toset(var.rg_names)
```

Conceptually:

```text
List:
["rg1", "rg2", "rg3"]

        ↓ toset()

Set:
{"rg1", "rg2", "rg3"}
```

A set does not maintain duplicate values.

For example:

```hcl
toset([
  "rg1",
  "rg1",
  "rg2"
])
```

results in unique elements:

```text
rg1
rg2
```

---

# 13. Different Regions – Map of Objects

If every Resource Group needs a different location, a simple `list(string)` is not enough because each item needs more than one attribute.

A map of objects can be used:

```hcl
variable "resource_groups" {
  type = map(object({
    name     = string
    location = string
  }))
}
```

Input:

```hcl
resource_groups = {
  prod = {
    name     = "prod-rg"
    location = "Central India"
  }

  dev = {
    name     = "dev-rg"
    location = "East US"
  }

  test = {
    name     = "test-rg"
    location = "West Europe"
  }
}
```

Resource:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = var.resource_groups

  name     = each.value.name
  location = each.value.location
}
```

Result:

```text
prod → prod-rg → Central India
dev  → dev-rg  → East US
test → test-rg → West Europe
```

This pattern is useful when each resource has multiple configurable attributes.

---

# 14. Practical Complete Example

## `variables.tf`

```hcl
variable "rg_names" {
  type = list(string)
}
```

## `terraform.tfvars`

```hcl
rg_names = [
  "angelone-rg1",
  "angelone-rg2",
  "angelone-rg3"
]
```

## `main.tf`

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = toset(var.rg_names)

  name     = each.value
  location = "Central India"
}
```

## Commands

```bash
terraform init
terraform plan
terraform apply
```

Terraform creates:

```text
angelone-rg1
angelone-rg2
angelone-rg3
```

---

# 15. When Should You Use `count`?

Use `count` when:

- Resources are almost identical.
- Simple numeric repetition is required.
- Index-based management is acceptable.
- You do not need meaningful resource keys.

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  count    = 3
  name     = "demo-rg-${count.index}"
  location = "Central India"
}
```

---

# 16. When Should You Use `for_each`?

Use `for_each` when:

- Resources have meaningful names/keys.
- Individual resources need independent management.
- You have a set of unique values.
- You have a map of configurations.
- You want resource identity based on keys/elements rather than numeric indexes.

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = {
    prod = "prod-rg"
    dev  = "dev-rg"
  }

  name     = each.value
  location = "Central India"
}
```

---

# 17. Interview Questions

## Q1. What is `count` in Terraform?

`count` is a meta-argument used to create multiple instances of a resource using a numeric count and `count.index`.

---

## Q2. What is `for_each`?

`for_each` is a Terraform meta-argument used to create multiple resource instances from a set or map.

---

## Q3. What is the difference between `count` and `for_each`?

`count` uses numeric indexes, whereas `for_each` uses set elements or map keys to identify resource instances.

---

## Q4. What is `count.index`?

`count.index` gives the current numeric index of a resource instance created using `count`.

---

## Q5. What are `each.key` and `each.value`?

For `for_each`:

```text
each.key   → current key
each.value → current value
```

---

## Q6. Why is `toset()` used?

`toset()` converts a list into a set so that the values can be used with `for_each`.

---

## Q7. Why can deleting an element from a list be problematic with `count`?

Because `count` identifies instances using numeric indexes. Removing an element from the middle can shift indexes and cause Terraform to detect changes in other resource instances.

---

# 18. Quick Revision

```text
List
  ↓
Collection of values

Map
  ↓
Key : Value collection

count
  ↓
Index-based
  ↓
count.index

for_each + Set
  ↓
Element-based
  ↓
each.value

for_each + Map
  ↓
Key/Value-based
  ↓
each.key
each.value

toset()
  ↓
List → Set
```

## 🔥 One-Line Rule

> **`count` = index-based resources**

> **`for_each` = key/element-based resources**

> **`each.key` = current key**

> **`each.value` = current value**

> **Map = key + value**

> **Set = unique elements**
