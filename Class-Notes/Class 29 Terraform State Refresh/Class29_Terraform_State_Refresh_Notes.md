# Class 29: Terraform State Refresh

## What is Terraform State Refresh?

Terraform State Refresh synchronizes the `terraform.tfstate` file with
the actual infrastructure running in the cloud provider.

## Key Components

-   **Configuration File (`.tf`)**: Desired infrastructure.
-   **State File (`terraform.tfstate`)**: Terraform's record of managed
    resources.
-   **Cloud Infrastructure**: Actual resources in Azure/AWS/GCP.

## Infrastructure Drift

Infrastructure drift occurs when the configuration, state file, and
actual infrastructure do not match.

## Zero Drift (Equilibrium)

When configuration, state file, and cloud resources are identical.

## Internal Flow of `terraform apply`

``` text
terraform refresh
        +
terraform plan
        +
terraform apply
```

## Scenario 1: New Resource Added to Configuration

-   Add resource in `.tf`
-   Not present in state or cloud
-   `terraform plan` shows `+ create`

## Scenario 2: Resource Removed from Configuration

-   Remove resource block from `.tf`
-   `terraform plan` shows `- destroy`
-   `terraform apply` deletes the resource

## Scenario 3: Resource Deleted Manually from Azure Portal

-   Resource exists in config but removed from portal
-   Refresh updates state
-   Plan recreates the missing resource

## Scenario 4: Resource Created Manually in Azure Portal

-   Resource exists only in portal
-   Not tracked by Terraform
-   No action unless imported

## Comparison Formula

``` text
Configuration - State = Plan
```

## Useful Commands

``` bash
terraform init
terraform plan
terraform apply
terraform refresh
terraform show
terraform state list
terraform state show <resource>
```

## Best Practices

-   Treat Terraform code as the source of truth.
-   Avoid manual portal changes.
-   Store state remotely.
-   Enable state locking.
-   Review plans before applying.
-   Use Git version control.

## Interview Questions

### What is Terraform State Refresh?

It synchronizes the state file with the real infrastructure.

### What is Infrastructure Drift?

A mismatch between configuration, state, and actual resources.

### What happens if a resource is removed from `.tf`?

Terraform plans to destroy it.

### What happens if a Terraform-managed resource is deleted manually?

Terraform recreates it if it still exists in the configuration.

### Why is the configuration the source of truth?

Terraform always tries to make infrastructure match the code.
