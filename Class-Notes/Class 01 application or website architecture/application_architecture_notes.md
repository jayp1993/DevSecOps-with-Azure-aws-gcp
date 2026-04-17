# Application / Website Architecture – Detailed Notes

## Architect Role
An Architect is responsible for designing the complete system. They decide:
- Application architecture (1-tier, 2-tier, 3-tier, microservices)
- Technology stack (Java, .NET, Node.js)
- Cloud platform (Azure, AWS, GCP)
- Security implementation
- Scalability (handling 10 users vs 1 million users)

---

## 1-Tier Architecture (Single Layer)
All components exist in a single system:
- Frontend (UI)
- Backend (Logic)
- Database

**Examples:**
- MS Excel
- Local desktop applications

**Flow:**
User → Same system

**Limitations:**
- Not scalable
- Poor multi-user support
- Not suitable for production

---

## 2-Tier Architecture (Client-Server)
Two layers:
- Client (UI + Logic)
- Database Server

**Flow:**
User → Client App → Database Server

**Examples:**
- Old banking systems
- Inventory systems

**Issues:**
- Security risk (direct DB access)
- Limited scalability
- Hard to maintain

---

## 3-Tier Architecture (Most Common)
Three layers:

### Presentation Layer (Frontend)
- React, Angular
- Runs in browser

### Application Layer (Backend)
- APIs and business logic
- Node.js, Java, .NET

### Data Layer (Database)
- SQL, MongoDB, PostgreSQL

**Flow:**
User → Frontend → Backend → Database

**Examples:**
- Amazon
- Netflix

**Benefits:**
- Secure
- Scalable
- Easy maintenance

---

## Restaurant Analogy
- Customer = Frontend
- Waiter = Backend
- Kitchen = Database

---

## 4-Tier Architecture
Adds one more layer:

- Client
- Web Server (Nginx, Apache)
- Application Server
- Database

**Flow:**
User → Web Server → App Server → Database

**Benefits:**
- Better performance
- Load distribution

---

## N-Tier Architecture (Advanced)
Multiple layers:

- Load Balancer
- API Gateway
- Microservices
- Cache (Redis)
- Message Queue (Kafka)
- Database

**Flow:**
User → LB → API Gateway → Services → DB

**Examples:**
- Netflix
- Amazon
- Swiggy

**Benefits:**
- High scalability
- Fault tolerance

---

## Monolithic vs Microservices

### Monolithic
- Single application
- Easy to build
- Hard to scale

### Microservices
- Multiple small services
- Independent deployment
- Highly scalable

---

## Cloud vs On-Premise

- On-Premise: Own data center
- Cloud: Azure, AWS, GCP

---

## Deployment Concept

Deployment means:
Taking code from source control (GitHub) and running it on a server.

---

## OS Usage in Production

- Linux: ~70%
- Windows: ~30%
- Mac: ~0%

---

## Real Example (MakeMyTrip)

- User searches flights
- Frontend calls API
- Backend processes request
- Database returns results
- Data displayed to user

---

## Interview Summary

- 1-tier → Single system
- 2-tier → Client-Server
- 3-tier → Frontend + Backend + Database
- 4-tier → Adds Web Server
- N-tier → Real-world architecture with microservices
