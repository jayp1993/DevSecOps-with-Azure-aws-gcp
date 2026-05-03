# 🚀 Azure Hierarchy + Entra ID (Class 12 Notes)

## 📌 1. Real-Time Azure Hierarchy (Production Example)

Consider a company: **Container.com**

### 🔷 Hierarchy Structure:

Tenant (Top Level)  
⬇  
Tenant Root Management Group  
⬇  
Management Groups:
- HR MG  
- Sales MG  
- Account MG  

⬇  
Subscriptions:
- HR Subscription  
- Sales Subscription  
- Account Subscription  

⬇  
Resource Groups  

⬇  
Resources:
- Virtual Machines (VM)
- Virtual Network (VNet)
- Database
- Storage Account
- Key Vault

### ✅ Key Concept:
- Hierarchy flows from **Top → Bottom**
- Policies and Access are **inherited downward**

---

## 📌 2. Why Azure Hierarchy is Used?

### 🔐 Access Management
- If you assign **Owner role at Management Group level**
→ Access is automatically applied to all:
  - Subscriptions
  - Resource Groups
  - Resources

👉 Reduces manual effort

---

### 📜 Policy Management
- Example: HR data must stay in India region  
- Apply policy at **HR MG level**

→ Automatically enforced on all child resources

👉 This ensures governance at scale

---

## 📌 3. Azure Roles (RBAC)

### 👁 Reader
- Can only view resources
- No modification allowed

---

### 🛠 Contributor
- Can create, update, delete resources
- Cannot assign access

---

### 👑 Owner
- Full access
- Can assign roles (access control)

👉 **Important:**
Owner = Contributor + Access Management

---

## 📌 4. Entra ID (Azure Active Directory)

### 🔷 Purpose:
Identity and Access Management

### 🔷 Components:
- Users
- Groups
- Roles

### 🔷 Important Roles:
- Global Administrator (highest privilege)
- User Administrator

---

## 📌 5. Important Facts

- A single directory supports up to **10,000 Management Groups**
- Maximum hierarchy depth = **6 levels**

### 🔥 Root Management Group:
- Cannot be deleted or moved
- All subscriptions are placed under it by default
- Only Global Admin can initially access it

👉 Root MG = Central control of Azure

---

## 📌 6. Real-Time Scenario (HR Head Access)

### 🧾 Situation:
A new HR Head (**Vinod**) joins the company  
You need to provide Azure access

---

### ✅ Step-by-Step Solution:

#### Step 1: Prerequisite
- Global Admin account required

---

#### Step 2: Create User in Entra ID
- Create user: Vinod

---

#### Step 3: Create Group
- Group Name: `HR Head GRP`

---

#### Step 4: Add User to Group
- Add Vinod to `HR Head GRP`

---

#### Step 5: Assign Role

- Scope: **HR Management Group**
- Role: **Owner**

---

### 🎯 Final Result:
Vinod gets:
- Full control over HR Management Group
- Access to all:
  - Subscriptions
  - Resource Groups
  - Resources under HR

👉 No need to assign access individually

---

## 📌 7. Real-Life Analogy

- Company = Mall 🏢  
- Management Group = Floors  
- Subscriptions = Shops  
- Resources = Items  

👉 HR Head = Floor Manager  
If access is given at floor level → all shops are automatically controlled

---

## 📌 8. Common Mistakes

❌ Assigning access at Subscription level only  
→ Not scalable  

❌ Assigning roles directly to users  
→ Always use Groups  

❌ Giving unnecessary access at Root MG  
→ Security risk  

---

## 📌 9. Final Summary

- Azure Hierarchy = Governance + Control  
- Structure: MG → Subscription → RG → Resource  
- Policies & Access → Inherited  
- Entra ID → Identity Management  
- Best Practice → Always use Group-based access

---

🔥 End of Notes