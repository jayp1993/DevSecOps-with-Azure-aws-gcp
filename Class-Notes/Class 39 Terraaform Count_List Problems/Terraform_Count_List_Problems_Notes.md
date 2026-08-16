# Terraform `count` + List — Complete Notes & Problems

## 1. What is `count`?

`count` is a Terraform meta-argument used to create multiple instances of a resource.

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3

  name     = "rg-${count.index}"
  location = "East US"
}
```

Terraform creates:

```text
azurerm_resource_group.rg[0]
azurerm_resource_group.rg[1]
azurerm_resource_group.rg[2]
```

The important object is:

```hcl
count.index
```

It starts from `0`.

---

# 2. What is a List?

A list is an ordered collection of values.

```hcl
variable "environments" {
  type = list(string)

  default = [
    "dev",
    "qa",
    "prod"
  ]
}
```

Indexes:

```text
index 0 → dev
index 1 → qa
index 2 → prod
```

Access values using:

```hcl
var.environments[0]
var.environments[1]
var.environments[2]
```

---

# 3. Using `count` with a List

This is one of the most common Terraform patterns.

```hcl
variable "environments" {
  type = list(string)

  default = [
    "dev",
    "qa",
    "prod"
  ]
}

resource "azurerm_resource_group" "rg" {
  count = length(var.environments)

  name     = "rg-${var.environments[count.index]}"
  location = "East US"
}
```

### How it works

```text
length(var.environments)
        ↓
        3
        ↓
count = 3
```

Terraform runs:

```text
count.index = 0 → dev
count.index = 1 → qa
count.index = 2 → prod
```

Resources:

```text
rg-dev
rg-qa
rg-prod
```

---

# 4. Important Functions

## `length()`

Returns the number of elements.

```hcl
length(["dev", "qa", "prod"])
```

Result:

```text
3
```

## List indexing

```hcl
var.environments[count.index]
```

Example:

```text
count.index = 0 → var.environments[0] → dev
count.index = 1 → var.environments[1] → qa
count.index = 2 → var.environments[2] → prod
```

---

# 5. Basic Azure Example

```hcl
variable "locations" {
  type = list(string)

  default = [
    "East US",
    "West US",
    "Central US"
  ]
}

resource "azurerm_resource_group" "rg" {
  count = length(var.locations)

  name     = "rg-${count.index}"
  location = var.locations[count.index]
}
```

Result:

```text
rg-0 → East US
rg-1 → West US
rg-2 → Central US
```

---

# 6. Better Naming with Two Lists

Suppose we have:

```hcl
variable "environments" {
  default = ["dev", "qa", "prod"]
}

variable "locations" {
  default = ["East US", "Central US", "West US"]
}
```

We can use:

```hcl
resource "azurerm_resource_group" "rg" {
  count = length(var.environments)

  name     = "rg-${var.environments[count.index]}"
  location = var.locations[count.index]
}
```

Result:

```text
rg-dev  → East US
rg-qa   → Central US
rg-prod → West US
```

### Important condition

Both lists should have matching indexes.

```text
environments[0] ↔ locations[0]
environments[1] ↔ locations[1]
environments[2] ↔ locations[2]
```

---

# 7. Problem #1 — Index Shift Problem

This is the most important `count + list` problem.

Initial list:

```hcl
[
  "dev",
  "qa",
  "prod"
]
```

Terraform creates:

```text
resource[0] → dev
resource[1] → qa
resource[2] → prod
```

Now remove `qa`:

```hcl
[
  "dev",
  "prod"
]
```

Terraform sees:

```text
resource[0] → dev
resource[1] → prod
```

Previously:

```text
resource[2] → prod
```

Now:

```text
resource[1] → prod
```

The resource address changed.

This is called the **index shift problem**.

---

# 8. Why Index Shift Can Be Dangerous

Suppose:

```text
resource[0] → dev
resource[1] → qa
resource[2] → prod
```

Remove the middle item:

```text
dev
prod
```

Terraform sees:

```text
resource[0] → dev
resource[1] → prod
```

Terraform does not identify resources by the value `dev`, `qa`, or `prod`.

It identifies them by their address:

```text
[0]
[1]
[2]
```

Therefore, removing an item from the middle can cause Terraform to propose changes to resources after that index.

---

# 9. Problem #2 — Removing the First Item

Initial:

```hcl
[
  "dev",
  "qa",
  "prod"
]
```

Addresses:

```text
[0] → dev
[1] → qa
[2] → prod
```

Remove `dev`:

```hcl
[
  "qa",
  "prod"
]
```

New addresses:

```text
[0] → qa
[1] → prod
```

Every remaining item shifted.

Potentially:

```text
dev  → removed
qa   → replaced/changed at [0]
prod → replaced/changed at [1]
```

The exact plan depends on the resource and provider, but the key issue is that resource identity is index-based.

---

# 10. Problem #3 — Reordering the List

Initial:

```hcl
[
  "dev",
  "qa",
  "prod"
]
```

Then:

```hcl
[
  "prod",
  "qa",
  "dev"
]
```

Indexes changed:

```text
0: dev  → prod
1: qa   → qa
2: prod → dev
```

Terraform sees changes at the indexed resource addresses.

For resources where an attribute change requires replacement, this can result in destroy/create operations.

---

# 11. Problem #4 — Duplicate Values

A list can contain duplicate values:

```hcl
[
  "dev",
  "dev",
  "prod"
]
```

With `count`, Terraform can still create three instances because the identity is:

```text
[0]
[1]
[2]
```

But duplicate logical names can cause problems if you use the list values as resource names:

```hcl
name = "rg-${var.environments[count.index]}"
```

You could end up with:

```text
rg-dev
rg-dev
rg-prod
```

Azure resource group names must be unique within the applicable naming scope, so this can fail.

---

# 12. Problem #5 — Empty List

Consider:

```hcl
variable "environments" {
  default = []
}
```

Then:

```hcl
count = length(var.environments)
```

becomes:

```text
count = 0
```

Terraform creates no resource instances.

This is normally safe and useful.

---

# 13. Problem #6 — Incorrect List Index

This can cause an error:

```hcl
var.environments[count.index + 1]
```

Suppose:

```text
environments = ["dev", "qa", "prod"]
```

The valid indexes are:

```text
0
1
2
```

When:

```text
count.index = 2
```

then:

```text
count.index + 1 = 3
```

Index `3` does not exist.

Terraform will produce an index-related error.

---

# 14. Problem #7 — Different List Lengths

Consider:

```hcl
variable "environments" {
  default = ["dev", "qa", "prod"]
}

variable "locations" {
  default = ["East US", "West US"]
}
```

Then:

```hcl
count = length(var.environments)
```

creates:

```text
count = 3
```

But:

```text
locations
0 → East US
1 → West US
2 → ❌ doesn't exist
```

This can cause an invalid index error.

---

# 15. How to Validate List Lengths

Use a validation condition.

```hcl
variable "environments" {
  type = list(string)
}

variable "locations" {
  type = list(string)

  validation {
    condition     = length(var.locations) == length(var.environments)
    error_message = "The number of locations must match the number of environments."
  }
}
```

However, cross-variable validation may not be appropriate in every Terraform version/configuration context. A safer design is often to avoid parallel lists altogether.

---

# 16. Problem #8 — Parallel Lists

This pattern is harder to maintain:

```hcl
environments = ["dev", "qa", "prod"]

locations = [
  "East US",
  "Central US",
  "West US"
]
```

The relationship depends on matching indexes:

```text
dev  ↔ East US
qa   ↔ Central US
prod ↔ West US
```

If someone changes one list but forgets the other, the mapping becomes incorrect.

---

# 17. Better Solution — List of Objects

Instead of parallel lists, use a list of objects:

```hcl
variable "resource_groups" {
  type = list(object({
    name     = string
    location = string
  }))

  default = [
    {
      name     = "dev"
      location = "East US"
    },
    {
      name     = "qa"
      location = "Central US"
    },
    {
      name     = "prod"
      location = "West US"
    }
  ]
}
```

Then:

```hcl
resource "azurerm_resource_group" "rg" {
  count = length(var.resource_groups)

  name     = "rg-${var.resource_groups[count.index].name}"
  location = var.resource_groups[count.index].location
}
```

This keeps the related values together.

---

# 18. Count + List of Objects

Example:

```hcl
variable "vms" {
  type = list(object({
    name = string
    size = string
  }))

  default = [
    {
      name = "vm-dev"
      size = "Standard_B2s"
    },
    {
      name = "vm-prod"
      size = "Standard_D2s_v5"
    }
  ]
}
```

Resource:

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  count = length(var.vms)

  name = var.vms[count.index].name
  size = var.vms[count.index].size
}
```

---

# 19. `count` Resource Address

With:

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3
}
```

Terraform addresses are:

```text
azurerm_resource_group.rg[0]
azurerm_resource_group.rg[1]
azurerm_resource_group.rg[2]
```

You can reference a particular instance:

```hcl
azurerm_resource_group.rg[0].name
```

Or all instances:

```hcl
azurerm_resource_group.rg[*].name
```

---

# 20. Useful Splat Expression

Suppose:

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3

  name     = "rg-${count.index}"
  location = "East US"
}
```

To get all names:

```hcl
azurerm_resource_group.rg[*].name
```

Result:

```text
[
  "rg-0",
  "rg-1",
  "rg-2"
]
```

---

# 21. `count.index` vs List Index

These are different concepts but commonly work together.

```hcl
count.index
```

is the index of the resource instance.

```hcl
var.environments[count.index]
```

uses that same index to retrieve an item from the list.

Example:

```text
count.index = 0
       ↓
var.environments[0]
       ↓
dev
```

---

# 22. `count` vs `for_each`

| Feature | `count` | `for_each` |
|---|---|---|
| Main input | Number | Map / Set |
| Identity | Numeric index | Key |
| Example | `[0]`, `[1]` | `["dev"]`, `["prod"]` |
| List support | Yes, using index | Convert list to set/map |
| Stable identity when middle item removed | ❌ | ✅ |
| Duplicate list values | Possible | Set removes duplicates |
| Best for | Similar/anonymous instances | Named logical instances |

---

# 23. When Should We Use `count`?

Use `count` when:

- Number of resources is the main requirement.
- Instances are effectively identical.
- Resources don't need meaningful keys.
- You don't expect frequent insertion/removal/reordering in the middle of a list.
- A boolean condition controls whether a resource exists.

Example:

```hcl
count = var.create_resource ? 1 : 0
```

This is a very common use of `count`.

---

# 24. When Should We Use `for_each`?

Prefer `for_each` when:

- Resources have meaningful names/keys.
- You need stable resource identity.
- You frequently add/remove individual items.
- You have a map of configuration.
- You have a list of objects that can be converted into a map.

Example:

```hcl
for_each = {
  for rg in var.resource_groups :
  rg.name => rg
}
```

---

# 25. Important Interview Question

### Why can `count` be problematic with lists?

Because `count` identifies resource instances by numeric indexes.

If an item is inserted, removed, or reordered in the middle of a list, the indexes of subsequent items can change.

Example:

```text
Before:
[0] dev
[1] qa
[2] prod

Remove qa:

After:
[0] dev
[1] prod
```

`prod` moved from `[2]` to `[1]`.

Therefore, Terraform may propose changes to resource instances whose indexes shifted.

---

# 26. Practical Problem

Given:

```hcl
variable "environments" {
  default = [
    "dev",
    "qa",
    "prod"
  ]
}
```

Create Resource Groups using `count`.

### Solution

```hcl
resource "azurerm_resource_group" "rg" {
  count = length(var.environments)

  name     = "rg-${var.environments[count.index]}"
  location = "East US"
}
```

Expected:

```text
rg-dev
rg-qa
rg-prod
```

---

# 27. Practical Problem — List of Objects

Given:

```hcl
resource_groups = [
  {
    name     = "dev"
    location = "East US"
  },
  {
    name     = "qa"
    location = "Central US"
  },
  {
    name     = "prod"
    location = "West US"
  }
]
```

Create Resource Groups using `count`.

### Solution

```hcl
resource "azurerm_resource_group" "rg" {
  count = length(var.resource_groups)

  name     = "rg-${var.resource_groups[count.index].name}"
  location = var.resource_groups[count.index].location
}
```

---

# 28. Practical Problem — Conditional Count

Create a resource only when the environment is production.

```hcl
variable "environment" {
  default = "prod"
}
```

Solution:

```hcl
resource "azurerm_resource_group" "prod" {
  count = var.environment == "prod" ? 1 : 0

  name     = "rg-prod"
  location = "West US"
}
```

If:

```text
environment = prod
```

then:

```text
count = 1
```

If:

```text
environment = dev
```

then:

```text
count = 0
```

---

# 29. Common Mistakes

### Mistake 1

```hcl
count = var.environments
```

Wrong if `var.environments` is a list.

Correct:

```hcl
count = length(var.environments)
```

---

### Mistake 2

```hcl
name = var.environments
```

Wrong because the entire list is being assigned.

Correct:

```hcl
name = var.environments[count.index]
```

---

### Mistake 3

Using a hard-coded count:

```hcl
count = 3
```

while the list contains a different number of elements.

Better:

```hcl
count = length(var.environments)
```

---

### Mistake 4

Using two independent lists without validating their lengths.

```hcl
environments = ["dev", "qa", "prod"]
locations    = ["East US", "West US"]
```

This can cause an invalid index.

---

# 30. Quick Revision

Remember:

```text
LIST
  ↓
length()
  ↓
count
  ↓
count.index
  ↓
list[count.index]
```

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  count = length(var.environments)

  name = "rg-${var.environments[count.index]}"
}
```

### Main Problem

```text
count
  ↓
numeric index
  ↓
list changes
  ↓
index shifts
  ↓
resource identity can change
```

### Better approach for named resources

```text
List of Objects
      ↓
Convert to Map
      ↓
for_each
      ↓
Stable Keys
```

---

# 31. Final Rule

> **Use `count` when instances are primarily quantity/index based. Use `for_each` when instances have meaningful, stable identities.**

For simple lists:

```hcl
count = length(var.list)
```

For accessing the current list item:

```hcl
var.list[count.index]
```

For named infrastructure objects:

```hcl
for_each = {
  for item in var.list :
  item.name => item
}
```

This distinction is extremely important for real-world Terraform projects and interviews.
