# Class 31: Terraform State Locking

> Based on the uploaded class notes.

## What is Terraform State?

Terraform stores infrastructure information in the **terraform.tfstate**
file. It maps Terraform configuration with real cloud resources.

The state file contains: - Resource IDs - Resource names - Current
configuration - Dependencies - Metadata

Without the state file, Terraform cannot determine what resources
already exist or what changes need to be applied.

------------------------------------------------------------------------

# Local State vs Remote State

## Local State

-   Stored on the local machine.
-   Difficult for teams.
-   No centralized backup.
-   Risk of accidental deletion.

## Remote State

Remote state stores the Terraform state file in cloud storage.

  Cloud          Backend
  -------------- --------------
  Azure          Blob Storage
  AWS            S3 Bucket
  Google Cloud   GCS Bucket

### Advantages

-   Centralized state management
-   State locking
-   Secure storage
-   Backup & redundancy
-   Easy collaboration

------------------------------------------------------------------------

# Terraform State Locking

State locking prevents multiple users from modifying the same state file
simultaneously.

Workflow:

1.  `terraform apply`
2.  Acquire state lock
3.  Refresh state
4.  Create execution plan
5.  Apply changes
6.  Update state file
7.  Release state lock

------------------------------------------------------------------------

# Azure State Locking

Azure Blob Storage uses **Blob Lease** for state locking.

    Acquire Lease
          ↓
    Terraform Apply
          ↓
    Update State
          ↓
    Release Lease

------------------------------------------------------------------------

# Multiple Users Scenario

Developer A starts:

``` bash
terraform apply
```

Developer B also starts:

``` bash
terraform apply
```

Result: - Developer A acquires the lock. - Developer B receives **Error
acquiring the state lock** until the first operation completes.

------------------------------------------------------------------------

# Common Error

    Error acquiring the state lock

Possible reasons: - System crash - Internet disconnect - User presses
Ctrl+C - Terraform process terminated unexpectedly

------------------------------------------------------------------------

# Solutions

## Option 1 -- Break Lease

Azure Portal:

Storage Account → Container → terraform.tfstate → **Break Lease**

## Option 2 -- Force Unlock

``` bash
terraform force-unlock <LOCK_ID>
```

Example:

``` bash
terraform force-unlock d6707a57-73ce-0eb8-efd1-1b4a4a3220a7
```

> Use `terraform force-unlock` only when you are certain no Terraform
> operation is running.

------------------------------------------------------------------------

# Best Practices

-   Use a remote backend.
-   Enable state locking.
-   Never edit `terraform.tfstate` manually.
-   Keep regular backups.
-   Maintain separate state files for Dev, QA, and Production.

------------------------------------------------------------------------

# Interview Questions

## What is Terraform State Locking?

A mechanism that prevents multiple users from updating the state file
simultaneously.

## Why is it required?

To prevent state corruption and inconsistent infrastructure.

## How does Azure implement state locking?

Using Blob Storage Lease.

## What happens when two users run `terraform apply` together?

The first user gets the lock. The second receives a state lock error
until the lock is released.

## How do you remove a stuck lock?

-   Break the Blob Lease.
-   Run `terraform force-unlock <LOCK_ID>`.

## Benefits of Remote State

-   Team collaboration
-   Secure storage
-   Backup & redundancy
-   State locking
-   Centralized management
