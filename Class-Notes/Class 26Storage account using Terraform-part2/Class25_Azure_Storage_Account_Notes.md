# Azure Storage Account, Redundancy & Access Tiers

## Introduction

Azure Storage Account is a storage service that provides highly available, secure, scalable, and durable storage for cloud applications.

A Storage Account can store:

* Blob Storage
* Azure Files
* Queue Storage
* Table Storage

---

# Azure Global Physical Infrastructure

Azure's physical infrastructure is organized in the following hierarchy:

```text
Geography
 └── Region
      └── Availability Zone
           └── Datacenter
                └── Rack
                     └── Disk
```

### Example

```text
United States (Geography)

├── East US
├── Central US
└── West US
```

Each region may contain multiple Availability Zones.

```text
East US Region

├── Zone 1
├── Zone 2
└── Zone 3
```

### India Example

```text
Asia Geography

├── Central India
├── South India
└── West India
```

---

# What is Redundancy?

Redundancy means maintaining multiple copies of data to protect against hardware failures, datacenter failures, or regional disasters.

### Benefits

* High Availability
* Data Durability
* Business Continuity
* Disaster Recovery
* Fault Tolerance

---

# Locally Redundant Storage (LRS)

## How It Works

Azure stores three copies of data within a single datacenter.

```text
Datacenter

Rack 1 → Copy 1
Rack 2 → Copy 2
Rack 3 → Copy 3
```

## Features

* Three copies of data
* Single datacenter protection
* Lowest storage cost
* Suitable for non-critical workloads

## SLA

**99.9% Availability**

## Use Cases

* Development environments
* Test environments
* Temporary workloads

---

# Zone Redundant Storage (ZRS)

## How It Works

Azure replicates data synchronously across three Availability Zones within the same region.

```text
Central India Region

Zone 1 → Copy 1
Zone 2 → Copy 2
Zone 3 → Copy 3
```

## Features

* Protection against zone failures
* High availability
* Synchronous replication

## SLA

**99.99% Availability**

## Use Cases

* Production applications
* Business-critical workloads

---

# Geo Redundant Storage (GRS)

## How It Works

Azure stores data in a primary region and asynchronously replicates it to a paired secondary region.

```text
Primary Region (Central India)
          │
          ▼
Asynchronous Replication
          │
          ▼
Secondary Region (South India)
```

## Features

* Six copies of data
* Three copies in primary region
* Three copies in secondary region
* Regional disaster protection

## Replication Type

**Asynchronous Replication**

## Secondary Region Access

* Passive by default
* Read access not available

## Use Cases

* Disaster recovery
* Long-term backups

---

# Read Access Geo Redundant Storage (RA-GRS)

RA-GRS provides all benefits of GRS plus read access to the secondary region.

## Features

* Secondary region is readable
* Supports reporting and analytics
* Improved business continuity

## Use Cases

* Reporting applications
* Read-intensive workloads

---

# Geo-Zone Redundant Storage (GZRS)

## How It Works

Data is synchronously replicated across Availability Zones in the primary region and asynchronously replicated to a secondary region.

```text
Primary Region

Zone 1
Zone 2
Zone 3

        │
        ▼
Asynchronous Replication

Secondary Region
```

## Features

* Zone failure protection
* Regional disaster protection
* Highest availability
* Maximum durability

## Replication Type

* Intra-region: Synchronous
* Cross-region: Asynchronous

## Use Cases

* Banking applications
* Healthcare systems
* Enterprise production workloads

---

# Redundancy Comparison

| Feature               | LRS  | ZRS    | GRS  | GZRS    |
| --------------------- | ---- | ------ | ---- | ------- |
| Copies of Data        | 3    | 3      | 6    | 6+      |
| Datacenter Protection | Yes  | Yes    | Yes  | Yes     |
| Zone Protection       | No   | Yes    | No   | Yes     |
| Region Protection     | No   | No     | Yes  | Yes     |
| Cost                  | Low  | Medium | High | Highest |
| Availability          | Good | Better | High | Highest |

---

# Synchronous Replication

## Process

```text
Write Request
     │
     ▼
Primary Copy
     │
     ▼
Secondary Copy
     │
     ▼
Success Response
```

## Advantages

* Near-zero data loss
* Immediate consistency
* High availability

## Used In

* LRS
* ZRS

---

# Asynchronous Replication

## Process

```text
Write Request
     │
     ▼
Primary Copy
     │
     ▼
Success Response
     │
     ▼
Background Replication
```

## Advantages

* Better performance
* Suitable for long-distance replication
* Cost-efficient

## Used In

* GRS
* RA-GRS
* GZRS

---

# Azure Storage Access Tiers

Azure Blob Storage supports multiple access tiers based on access frequency.

---

## Hot Tier

### Best For

Frequently accessed data.

### Examples

* Website images
* Active project files
* Daily accessed documents

### Cost

* High storage cost
* Low access cost

---

## Cool Tier

### Best For

Infrequently accessed data.

### Examples

* Monthly reports
* Backup files
* Archived project data

### Cost

* Lower storage cost
* Higher access cost than Hot Tier

---

## Cold Tier

### Best For

Rarely accessed data.

### Examples

* Compliance records
* Historical archives
* Long-term retention data

### Cost

* Lowest storage cost
* Highest retrieval cost

---

# Interview Questions

### 1. What is Azure Storage Account?

Azure Storage Account is a cloud storage service that provides Blob, File, Queue, and Table storage services.

### 2. What is Redundancy?

Redundancy is the process of maintaining multiple copies of data to ensure availability and durability.

### 3. What is the difference between LRS and ZRS?

* LRS stores data within a single datacenter.
* ZRS stores data across multiple Availability Zones.

### 4. What is the difference between GRS and GZRS?

* GRS protects against regional failures.
* GZRS protects against both zone and regional failures.

### 5. Which redundancy option provides the highest availability?

GZRS (Geo-Zone Redundant Storage)

### 6. What is the SLA of ZRS?

99.99%

### 7. Which redundancy option is the cheapest?

LRS (Locally Redundant Storage)

### 8. Which redundancy options support regional disaster recovery?

* GRS
* RA-GRS
* GZRS

### 9. What is the difference between synchronous and asynchronous replication?

* Synchronous replication writes data to multiple locations before confirming success.
* Asynchronous replication confirms success first and replicates later in the background.

### 10. What are the Azure Storage Access Tiers?

* Hot Tier
* Cool Tier
* Cold Tier

---

# Key Exam and Interview Notes

* LRS = Single Datacenter Protection
* ZRS = Availability Zone Protection
* GRS = Regional Protection
* RA-GRS = Regional Protection + Read Access
* GZRS = Zone + Regional Protection
* Hot Tier = Frequently Accessed Data
* Cool Tier = Less Frequently Accessed Data
* Cold Tier = Rarely Accessed Data
* ZRS SLA = 99.99%
* LRS SLA = 99.9%
* GZRS provides the highest availability and durability.
