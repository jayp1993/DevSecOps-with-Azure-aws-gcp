# 🚀 Class 10 – Identity & Access Management (IAM)

---

# 🧠 1. Cloud Access Flow (High-Level Understanding)

In any cloud platform (Azure, AWS, GCP), the access flow works like this:

- User → Portal / CLI / API  
- → Authentication (Who are you?)  
- → Authorization (What can you access?)  
- → Resource Access  

### Tools used:
- Manual: Azure Portal (portal.azure.com)  
- Automation: Azure CLI, PowerShell  
- IaC: Terraform, Bicep, ARM Templates  

---

# ⚙️ 2. Manual vs Automation (Pizza Analogy 🍕)

## 🔹 Manual Approach (Portal)
- Login to portal  
- Search VM  
- Configure network  
- Attach disk  
- Click create  

👉 Everything is done manually (click-by-click)  
👉 Time-consuming and error-prone  

---

## 🔹 Imperative Automation (Step-by-step control)

You tell the system **each step**.

👉 Example:
- Create VM  
- Attach network  
- Add disk  

### Tools:
- Azure CLI  
- Azure PowerShell  
- AWS CLI  
- GCP CLI  

👉 Control is in your hands  

---

## 🔹 Declarative Automation (Final state approach)

You define **what you want**, not how.

👉 Example:
- “I want a VM with network and disk”

👉 Tool decides:
- How to create  
- What sequence to follow  

### Tools:
- Terraform  
- Bicep  
- ARM Templates  
- CloudFormation  

👉 Control is with the tool  

---

# ⚡ 3. Imperative vs Declarative

| Type        | Approach                | Control        |
|------------|------------------------|---------------|
| Imperative | Step-by-step commands  | User          |
| Declarative| Define final state     | Tool          |

---

# 🏗️ 4. Infrastructure as Code (IaC)

👉 Managing infrastructure using code instead of manual work  

## 🔹 Types:

### Cloud Native:
- ARM Template (Azure)  
- Bicep  
- CloudFormation (AWS)  

### Open Source (Multi-cloud):
- Terraform  
- Pulumi  
- Crossplane  

---

# 🔐 5. IAM (Identity & Access Management)

👉 IAM = **Who you are + What you can do**

## 🔹 Two Core Components:
1. Authentication → Identity verification  
2. Authorization → Permission control  

---

# 🥟 6. Token-Based Authentication (Analogy)

## Real-life Example

1. You go to counter  
2. Pay money  
3. Get a token 🎟️  
4. Show token to collect item  

## Cloud Mapping:

| Real Life | Cloud |
|----------|------|
| Person | User |
| Guard | IAM System |
| Money | Credentials |
| Token | JWT Token |
| Resource | Service |

## Cloud Flow:

1. Enter username & password  
2. Verified by Entra ID  
3. JWT Token generated  
4. Token used to access resources  

---

# 🎫 7. JWT Token

- A secure digital token  
- Used for authentication  
- Sent with every request  

---

# 🧑‍💼 8. Role-Based Access Control (RBAC)

👉 Access is assigned based on roles

## Roles:
- Owner → Full access  
- Contributor → Modify access  
- Reader → Read-only access  

---

# 🧾 9. Azure Account Concept

- Entry point to Azure  
- Requires credit/debit card  
- Free credits available (trial)  

👉 Account creator = Owner  

## Important:
- Entra ID is automatically created with Azure account  

---

# 🏢 10. Entra ID (Azure AD)

👉 Identity provider of Azure

## Responsibilities:
- Authentication  
- Authorization  
- Identity management  

---

# 🌳 11. Azure Hierarchy

```
Root Management Group
   ↓
Management Group
   ↓
Subscription
   ↓
Resource Group
   ↓
Resources
```

## Explanation:

### Root Management Group
- Top-level container  

### Management Group
- Organizes subscriptions  

### Subscription
- Billing boundary  

### Resource Group
- Logical grouping  

### Resources
- VM, Storage, DB, etc.  

---

# 🔥 12. Example Structure

```
Root MG
 ├── HR MG
 │     └── HR Subscription
 │           └── HR Resource Group
 │                └── Resources
 │
 ├── Account MG
 │     └── Account Subscription
 │
 └── Tech MG
       ├── Dev Subscription
       └── Infra Subscription
```

---

# 🎯 13. Interview Questions

**Q1: What is IAM?**  
A: Identity + Access control system

**Q2: Authentication vs Authorization?**  
A: Authentication = Who you are  
   Authorization = What you can access

**Q3: Imperative vs Declarative?**  
A: Imperative = Steps  
   Declarative = Final state

**Q4: What manages IAM in Azure?**  
A: Entra ID

**Q5: What is RBAC?**  
A: Role-based permission system

**Q6: What is Terraform?**  
A: Multi-cloud IaC tool

---

# 🧠 Final Summary

- IAM = Identity + Access control  
- Token = Access pass  
- RBAC = Role-based permissions  
- Entra ID = Azure IAM engine  
- Terraform = IaC tool

---

# 💡 Teaching Tip

👉 IAM = Gate → Token → Resource access
