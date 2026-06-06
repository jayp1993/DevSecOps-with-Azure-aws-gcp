# Class 25 Notes: Azure Storage Account using Terraform

## Azure Storage Account
A Storage Account is a Microsoft Azure service used to store different types of data in the cloud.

### Storage Services
- Blob Storage – Unstructured Data
- File Share – Shared Files
- Table Storage – Structured/NoSQL Data
- Queue Storage – Messages

---

## Types of Data

### Structured Data
Stored in rows and columns.

| CustomerID | Name | Email |
|------------|------|-------|
| 1 | Ram | ram@gmail.com |
| 2 | John | john@gmail.com |

### Unstructured Data
- Images
- Videos
- Audio
- Documents
- Backups

---

## Blob Storage

Blob = Binary Large Object

Used for:
- Images
- Videos
- Backups
- Log Files
- ISO Files

### Types of Blobs

#### Block Blob
- Images
- Videos
- Documents
- Backups

#### Page Blob
- Azure VM Disks
- OS Disk
- Data Disk

#### Append Blob
- Application Logs
- Monitoring Logs
- Audit Logs

---

## Blob Container

Container acts like a folder inside a Storage Account.

Example:

Storage Account
- photos
- videos
- backups

---

## Azure File Share

Uses SMB (Server Message Block) protocol.

Benefits:
- Shared Storage
- Centralized Access
- Easy Migration
- Common File Repository

Use Cases:
- Shared Documents
- Application Files
- User Profiles

---

## Azure Table Storage

NoSQL storage service.

Features:
- Schema-less
- High Scalability
- Fast Retrieval

Use Cases:
- Customer Information
- IoT Data
- Metadata Storage

---

## Azure Queue Storage

Used for asynchronous communication.

FIFO = First In First Out

Use Cases:
- Email Processing
- SMS Notifications
- Order Processing
- Background Jobs

---

## Storage Performance Types

### Standard Storage
- General Purpose
- Blob
- File Share
- Table
- Queue
- Lower Cost

### Premium Storage
- High Performance SSD
- Block Blob
- Page Blob
- File Share
- Higher Cost

---

## Storage Endpoints

| Service | Endpoint |
|----------|----------|
| Blob | https://account.blob.core.windows.net |
| File Share | https://account.file.core.windows.net |
| Queue | https://account.queue.core.windows.net |
| Table | https://account.table.core.windows.net |

---

## Terraform Example

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "storage-rg"
  location = "Central India"
}
```

```hcl
resource "azurerm_storage_account" "sa" {
  name                     = "jpstorageaccount001"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

---

## Interview Questions

### What are the services available in Azure Storage Account?
- Blob Storage
- File Share
- Table Storage
- Queue Storage

### Which Blob type is used for Azure VM disks?
Page Blob

### Which Blob type is used for logs?
Append Blob

### Which protocol is used by Azure File Share?
SMB

### Which storage service follows FIFO?
Queue Storage

### Which storage service is NoSQL?
Table Storage

---

## Production Scenario

E-Commerce Application:

- Product Images → Blob Storage
- Shared Files → Azure File Share
- Customer Metadata → Table Storage
- Order Processing → Queue Storage

### Summary

Storage Account = Parent Service

- Blob → Unstructured Data
- File Share → SMB Shared Storage
- Table → NoSQL Data
- Queue → FIFO Messaging
- Standard → General Purpose
- Premium → High Performance
