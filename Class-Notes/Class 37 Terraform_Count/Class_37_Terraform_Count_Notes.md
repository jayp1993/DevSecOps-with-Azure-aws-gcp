# 🚀 Class 37: Terraform Count

> **Source:** Class 37 PDF (Terraform Count)

------------------------------------------------------------------------

# What is `count`?

`count` is a **Meta-Argument** in Terraform used to create multiple
identical resources from a single resource block.

``` hcl
resource "azurerm_resource_group" "rg" {
  count    = 3
  name     = "cloud-rg-${count.index}"
  location = "Central India"
}
```

Terraform creates:

-   cloud-rg-0
-   cloud-rg-1
-   cloud-rg-2

------------------------------------------------------------------------

# Why use Count?

-   Reduce repetitive code
-   Automate resource creation
-   Easy to scale infrastructure
-   Easy maintenance

------------------------------------------------------------------------

# Meta-Arguments

-   `count`
-   `for_each`
-   `depends_on`
-   `provider`
-   `lifecycle`

------------------------------------------------------------------------

# Count Rules

✅ Accepts only whole numbers.

Valid:

``` text
0
1
2
10
100
```

Invalid:

``` text
1.5
-1
true
"abc"
```

------------------------------------------------------------------------

# `count.index`

Terraform automatically generates `count.index`.

Example:

``` hcl
count = 4
```

Indexes:

``` text
0
1
2
3
```

------------------------------------------------------------------------

# Dynamic Naming

``` hcl
resource "azurerm_resource_group" "rg" {
  count = 3

  name     = "project-rg-${count.index}"
  location = "Central India"
}
```

Output:

``` text
project-rg-0
project-rg-1
project-rg-2
```

------------------------------------------------------------------------

# String Interpolation

``` hcl
"${count.index}"
```

Example:

``` hcl
name = "cloud-rg-${count.index}"
```

------------------------------------------------------------------------

# Resource Address

Terraform stores resources as:

``` text
azurerm_resource_group.rg[0]
azurerm_resource_group.rg[1]
azurerm_resource_group.rg[2]
```

------------------------------------------------------------------------

# Array vs Index

``` text
["Ram","Shyam","Tony"]

0 -> Ram
1 -> Shyam
2 -> Tony
```

Terraform follows the same zero-based indexing.

------------------------------------------------------------------------

# Real-Time Example

``` hcl
resource "azurerm_resource_group" "rg" {
  count = 5

  name     = "project-rg-${count.index}"
  location = "Central India"
}
```

Creates five Resource Groups automatically.

------------------------------------------------------------------------

# Advantages

-   Less code
-   Faster deployment
-   Easy scaling
-   Better readability
-   Automation

------------------------------------------------------------------------

# Limitations

Use `count` only when resources are almost identical.

For different configurations, use **for_each**.

------------------------------------------------------------------------

# Count vs for_each

  Count                for_each
  -------------------- --------------------------------
  Number Based         Key Based
  Uses `count.index`   Uses `each.key` / `each.value`
  Same Resources       Different Resources

------------------------------------------------------------------------

# Interview Questions

### What is Count?

A Meta-Argument used to create multiple instances of the same resource.

### What is `count.index`?

An automatically generated index starting from **0**.

### What values does Count accept?

Only whole numbers.

### When should you use Count?

When creating multiple identical resources.

### When should you use for_each?

When every resource has different values.

------------------------------------------------------------------------

# Key Takeaways

-   `count` is a Meta-Argument.
-   Starts indexing from **0**.
-   Use `${count.index}` for dynamic names.
-   Great for identical resources.
-   Prefer `for_each` for unique resources.

------------------------------------------------------------------------

## Next Topic

**Terraform Count + List**
