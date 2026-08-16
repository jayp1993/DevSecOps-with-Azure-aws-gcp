# Terraform `for_each` + List — Complete Notes

## 1. What is `for_each`?

Terraform `for_each` is a **meta-argument** used to create multiple resource instances from a collection.

Instead of writing the same resource multiple times, we can use `for_each`.

### Without `for_each`

```hcl
resource "azurerm_resource_group" "rg1" {
  name     = "rg-dev"
  location = "East US"
}

resource "azurerm_resource_group" "rg2" {
  name     = "rg-prod"
  location = "West US"
}
```

### With `for_each`

```hcl
variable "resource_groups" {
  default = {
    dev  = "East US"
    prod = "West US"
  }
}

resource "azurerm_resource_group" "rg" {
  for_each = var.resource_groups

  name     = "rg-${each.key}"
  location = each.value
}
```

Terraform creates:

```text
rg-dev  → East US
rg-prod → West US
```

---

## 2. `for_each` Syntax

```hcl
resource "resource_type" "resource_name" {
  for_each = collection

  argument = each.value
}
```

`for_each` commonly works with:

- Map
- Set

A common mistake is trying to directly use a **list** with `for_each`.

---

## 3. `each.key` and `each.value`

Inside a `for_each` resource, Terraform provides:

### `each.key`

Returns the key of the current map/set item.

### `each.value`

Returns the value of the current item.

Example:

```hcl
variable "users" {
  default = {
    admin = "Jay"
    dev   = "Rahul"
  }
}

resource "example" "user" {
  for_each = var.users

  name = each.value
}
```

For `admin`:

```text
each.key   = admin
each.value = Jay
```

For `dev`:

```text
each.key   = dev
each.value = Rahul
```

---

## 4. Terraform List

A list is an **ordered collection**.

```hcl
variable "locations" {
  type = list(string)

  default = [
    "East US",
    "West US",
    "Central US"
  ]
}
```

List indexes start from `0`.

```text
locations[0] = East US
locations[1] = West US
locations[2] = Central US
```

---

## 5. Can We Directly Use a List with `for_each`?

**No.**

This is invalid:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = var.locations

  name     = "rg-${each.value}"
  location = each.value
}
```

`for_each` expects a **map or set**, not a list.

---

## 6. Convert List into Set

We can convert a list into a set using:

```hcl
toset()
```

Example:

```hcl
variable "locations" {
  default = [
    "East US",
    "West US",
    "Central US"
  ]
}

resource "azurerm_resource_group" "rg" {
  for_each = toset(var.locations)

  name     = "rg-${each.value}"
  location = each.value
}
```

### Important

When using a set:

```hcl
each.key
```

and

```hcl
each.value
```

represent the same value.

Example:

```text
each.key   = East US
each.value = East US
```

A set is unordered and removes duplicate values.

---

## 7. List vs Set

| Feature | List | Set |
|---|---|---|
| Ordered | Yes | No |
| Duplicate values | Allowed | Not allowed |
| Indexing | Yes | No |
| Directly usable with `for_each` | No | Yes |
| Example | `["dev","prod"]` | `toset(["dev","prod"])` |

---

## 8. Convert List into Map

Another common approach is converting a list into a map.

Suppose:

```hcl
variable "environments" {
  default = [
    "dev",
    "test",
    "prod"
  ]
}
```

We can use:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = {
    for env in var.environments :
    env => env
  }

  name     = "rg-${each.key}"
  location = "East US"
}
```

The resulting map is conceptually:

```text
dev  → dev
test → test
prod → prod
```

---

## 9. `for_each` with List of Objects

This is extremely important in real-world projects.

### Variable

```hcl
variable "resource_groups" {
  type = list(object({
    name     = string
    location = string
  }))

  default = [
    {
      name     = "rg-dev"
      location = "East US"
    },
    {
      name     = "rg-prod"
      location = "West US"
    }
  ]
}
```

We cannot directly use:

```hcl
for_each = var.resource_groups
```

Instead, convert the list into a map:

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = {
    for rg in var.resource_groups :
    rg.name => rg
  }

  name     = each.value.name
  location = each.value.location
}
```

Now:

```text
each.key
    ↓
rg-dev

each.value
    ↓
{
  name     = "rg-dev"
  location = "East US"
}
```

---

## 10. Why Convert List to Map?

Suppose we have:

```hcl
[
  {
    name = "dev"
  },
  {
    name = "prod"
  }
]
```

Terraform's `for_each` needs unique identifiers.

So we create:

```hcl
{
  dev  = {...}
  prod = {...}
}
```

This gives every resource a stable identity.

---

## 11. `count` vs `for_each`

### Using `count`

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3

  name     = "rg-${count.index}"
  location = "East US"
}
```

Resources:

```text
azurerm_resource_group.rg[0]
azurerm_resource_group.rg[1]
azurerm_resource_group.rg[2]
```

### Using `for_each`

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = toset(["dev", "test", "prod"])

  name     = "rg-${each.value}"
  location = "East US"
}
```

Resources:

```text
azurerm_resource_group.rg["dev"]
azurerm_resource_group.rg["test"]
azurerm_resource_group.rg["prod"]
```

---

## 12. Why `for_each` Can Be Better Than `count`

Consider:

```hcl
["dev", "test", "prod"]
```

With `count`:

```text
0 → dev
1 → test
2 → prod
```

If we remove `test`:

```hcl
["dev", "prod"]
```

The indexes become:

```text
0 → dev
1 → prod
```

The identity of `prod` changes from:

```text
rg[2]
```

to:

```text
rg[1]
```

This can cause unnecessary resource changes.

With `for_each`:

```text
rg["dev"]
rg["test"]
rg["prod"]
```

Removing `test` only removes:

```text
rg["test"]
```

The identities of `dev` and `prod` remain stable.

---

# 13. Real-World Azure Example

## `terraform.tfvars`

```hcl
resource_groups = [
  {
    name     = "rg-dev"
    location = "East US"
  },
  {
    name     = "rg-test"
    location = "Central US"
  },
  {
    name     = "rg-prod"
    location = "West US"
  }
]
```

## `variables.tf`

```hcl
variable "resource_groups" {
  type = list(object({
    name     = string
    location = string
  }))
}
```

## `main.tf`

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = {
    for rg in var.resource_groups :
    rg.name => rg
  }

  name     = each.value.name
  location = each.value.location
}
```

Terraform creates:

```text
rg-dev  → East US
rg-test → Central US
rg-prod → West US
```

---

# 14. Important Terraform Concepts

### List

```hcl
["dev", "test", "prod"]
```

Ordered collection.

### Set

```hcl
toset(["dev", "test", "prod"])
```

Unordered collection with unique values.

### Map

```hcl
{
  dev  = "East US"
  prod = "West US"
}
```

Key-value collection.

### Object

```hcl
{
  name     = "rg-dev"
  location = "East US"
}
```

Structured collection with defined attributes.

---

# 15. Important Interview Questions

### Q1. What is `for_each`?

`for_each` is a Terraform meta-argument used to create multiple instances of a resource from a map or set.

### Q2. Can we use a list directly with `for_each`?

No. A list must generally be converted to a set or map.

### Q3. How do you convert a list to a set?

```hcl
toset(var.list)
```

### Q4. What is `each.key`?

It represents the key of the current item.

### Q5. What is `each.value`?

It represents the value of the current item.

### Q6. When should we prefer `for_each` over `count`?

Use `for_each` when resources have meaningful, stable identities such as:

```text
dev
test
prod
```

rather than only numeric indexes:

```text
0
1
2
```

---

# 16. Most Important Real-World Pattern

### Simple List

```text
LIST
  ↓
toset()
  ↓
for_each
  ↓
each.value
```

### List of Objects

```text
LIST OF OBJECTS
       ↓
 for expression
       ↓
      MAP
       ↓
   for_each
       ↓
each.key / each.value
```

The most useful pattern to remember is:

```hcl
for_each = {
  for item in var.list :
  item.name => item
}
```

Then:

```hcl
each.key
```

→ unique identifier

```hcl
each.value
```

→ complete object

---

# 17. Quick Revision

```text
for_each
   │
   ├── Map      → each.key + each.value
   │
   └── Set      → each.key + each.value
                    │
                    └── key/value are same
```

For a list:

```text
List
 │
 ├── Simple values
 │      ↓
 │    toset()
 │      ↓
 │   for_each
 │
 └── Objects
        ↓
   Convert to Map
        ↓
     for_each
```

### Golden Rule

> **`for_each` works with Map or Set. For a List, convert it to a Set or Map depending on the use case.**

---

## Practice Exercise

Create 3 Azure Resource Groups using a list of objects:

```hcl
resource_groups = [
  {
    name     = "rg-dev"
    location = "East US"
  },
  {
    name     = "rg-qa"
    location = "Central US"
  },
  {
    name     = "rg-prod"
    location = "West US"
  }
]
```

Requirements:

1. Define the variable using `list(object(...))`.
2. Convert the list into a map using a `for` expression.
3. Create Resource Groups using `for_each`.
4. Verify the resources using `terraform plan`.
5. Remove `rg-qa` and check how Terraform handles the change.
