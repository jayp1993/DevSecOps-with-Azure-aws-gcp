# Class 27 -- Terraform Dependencies Notes

## Terraform Dependencies

Terraform Dependency ensures resources are created in the correct order.

Example: - Resource Group - Storage Account

## Terraform Commands

### terraform init

Initializes the project and downloads providers.

``` bash
terraform init
```

### terraform fmt

Formats Terraform code.

``` bash
terraform fmt
```

### terraform plan

Shows execution plan without deploying.

``` bash
terraform plan
```

### terraform apply

Deploys infrastructure after confirmation.

``` bash
terraform apply
```

## How Terraform Works

1.  Looks in the current working folder.
2.  Reads all `.tf` files.
3.  Finds all `resource` blocks.
4.  Builds a dependency graph.
5.  Creates resources in the correct order.

## Implicit Dependency

Terraform automatically detects dependencies through references.

``` hcl
resource "azurerm_resource_group" "rg" {
  name     = "sonu-rg"
  location = "West Europe"
}

resource "azurerm_storage_account" "storage" {
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
}
```

Benefits: - No duplicate values - Automatic ordering - Easier
maintenance

## Explicit Dependency

Use `depends_on` when no direct reference exists.

``` hcl
resource "azurerm_storage_account" "storage" {
  depends_on = [
    azurerm_resource_group.rg
  ]
}
```

## Interview Question

**Q:** If Resource Group is created but Storage Account creation fails,
what happens on the next `terraform apply`?

**A:** Terraform checks the state and creates only the missing Storage
Account.

------------------------------------------------------------------------

Generated from the uploaded Class 27 Terraform Dependencies diagram.
