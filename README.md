![Image](https://cdn.prod.website-files.com/644656ba41efb6b601e93ca6/66bba48876d772de51146dcc_AD_4nXfdO3mmGzi9KQaLdSPG8IRLO3na9SvYmzH8JnVysolnw5ohlqCELpMmPwwzr4s4Kfq1BIG7U4jDt5o6SmI-EiJrIcTg7FKSm98ii_lM70iMxxHKFO7Q-ENaOlI_zyNZA5Jn2WJGfzTCBGw4hmnVB340qkM.png)

![Image](https://miro.medium.com/0%2A85NPhj05H5U4aNuo.png)

![Image](https://docs.aws.amazon.com/images/organizations/latest/userguide/images/scp_deny_1.png)

![Image](https://d2908q01vomqb2.cloudfront.net/c5b76da3e608d34edb07244cd9b875ee86906328/2022/02/02/representative-OU-structure-for-an-FSI-banking-customer-750x630.png)

---

# 🇮🇳 IT Operations Analyst (AWS / Azure)

**Practice & Interview Preparation Playbook**

---

## 🔹 MODULE 1: Cloud Fundamentals for IT Operations

### 🔑 Key Points to Remember

* Ops ≠ Dev → **Stability, Compliance, SLA, RCA**
* Shared Responsibility Model (AWS vs Azure)
* Control Plane vs Data Plane
* Regions, AZs, Subscriptions, Accounts
* Tagging = Cost + Governance backbone

### 🛠 Practice Tasks

* Create Azure Subscription / AWS Account
* Enable activity logs & audit logs
* Apply mandatory tags using policy
* Simulate an outage → write RCA

### 🎯 Interview Questions

**Q1. Difference between Azure Subscription and AWS Account?**
**A:**

* Azure Subscription = billing + resource boundary under a tenant
* AWS Account = isolated resource container under AWS Organizations

**Q2. What is SLA and how do you handle SLA breach?**
**A:**
Identify incident → mitigate → communicate → RCA → preventive action

---

## 🔹 MODULE 2: Identity & Access Management (MANDATORY)

### 🔑 Key Points

* **Zero Trust**
* Least Privilege
* RBAC vs ABAC
* Human vs Machine identity
* MFA everywhere

### ☁ Services to Know

**Azure**

* Azure AD (Entra ID)
* RBAC
* Privileged Identity Management (PIM)

**AWS**

* IAM Users, Roles, Policies
* STS
* Permission Boundaries

### 🛠 Practice

```bash
# AWS: Create IAM role for EC2
aws iam create-role \
--role-name EC2ReadOnlyRole \
--assume-role-policy-document file://trust.json
```

```hcl
# Azure RBAC via Terraform
resource "azurerm_role_assignment" "reader" {
  role_definition_name = "Reader"
  principal_id         = azurerm_user_assigned_identity.example.principal_id
  scope                = azurerm_resource_group.rg.id
}
```

### 🎯 Interview Q&A

**Q. IAM Role vs IAM User?**
**A:** Roles are temporary & secure; Users are long-term credentials (avoid)

**Q. How do you prevent privilege escalation?**
**A:** Permission boundaries, PIM, SCPs, approval workflows

---

## 🔹 MODULE 3: Cloud Governance & Policy-as-Code ⭐ CORE

### 🔑 Key Concepts

* Guardrails, not roadblocks
* Deny > Audit > Modify
* Policies must be **non-disruptive**
* Exception management is critical

### ☁ Services

**Azure**

* Azure Policy
* Initiative (Policy Set)
* Management Groups
* Policy Insights

**AWS**

* Organizations
* Service Control Policies (SCP)
* Resource Control Policies (RCP)
* AWS Config Rules

### 🛠 Practice

**Azure Policy – Deny Public IP**

```json
{
  "if": {
    "field": "Microsoft.Network/publicIPAddresses/publicIPAllocationMethod",
    "equals": "Static"
  },
  "then": {
    "effect": "deny"
  }
}
```

**AWS SCP – Block IAM Users**

```json
{
  "Effect": "Deny",
  "Action": "iam:CreateUser",
  "Resource": "*"
}
```

### 🎯 Interview Q&A

**Q. SCP vs IAM Policy?**
**A:**

* SCP = guardrail at Org/OU level
* IAM = permissions inside account

**Q. How do you test policy impact?**
**A:**

* Azure What-If
* Compliance reports
* Deploy in audit mode first

---

## 🔹 MODULE 4: Infrastructure as Code (Terraform Preferred)

### 🔑 Key Points

* Idempotency
* State management (Remote backend)
* Modules = reusability
* Drift detection

### ☁ Tools

* Terraform
* AzureRM Provider
* AWS Provider
* Terraform Cloud / S3 Backend

### 🛠 Practice

```hcl
terraform {
  backend "s3" {
    bucket = "tf-state-prod"
    key    = "governance/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

### 🎯 Interview Q&A

**Q. Terraform vs ARM / CloudFormation?**
**A:** Terraform is cloud-agnostic, modular, and widely adopted

**Q. How do you handle secrets in Terraform?**
**A:** Key Vault / Secrets Manager + environment variables

---

## 🔹 MODULE 5: CI/CD for Policy & IaC Deployment

### 🔑 Key Points

* Policy as code lives in Git
* PR-based approvals
* Automated compliance
* Rollback strategy

### ☁ Services

* Azure DevOps Pipelines
* GitHub Actions
* AWS CodePipeline

### 🛠 Practice

```yaml
# Azure DevOps – Terraform pipeline
steps:
- task: TerraformCLI@0
  inputs:
    command: 'apply'
```

### 🎯 Interview Q&A

**Q. How do you troubleshoot pipeline failures?**
**A:** Logs → Unit tests → Validate syntax → Provider auth

**Q. What tests do you run for policies?**
**A:** What-If, dry run, compliance scan

---

## 🔹 MODULE 6: Incident, Change & ITIL (Ops Mindset)

### 🔑 Key Points

* Incident ≠ Change
* CAB approval
* Change windows
* Post Incident Review (PIR)

### 🎯 Interview Q&A

**Q. How do you handle a P1 incident?**
**A:**
Contain → communicate → mitigate → RCA → prevention

**Q. Why ITIL matters in cloud?**
**A:** Cloud failures still affect business SLAs

---

## 🔹 MODULE 7: Scripting & Automation

### 🔑 Languages

* PowerShell (Azure)
* Python (AWS automation)
* Bash

### 🛠 Practice

```powershell
Get-AzPolicyAssignment
```

```python
import boto3
iam = boto3.client('iam')
iam.list_roles()
```

---

## 🔹 MODULE 8: Documentation & Cross-Team Collaboration

### 🔑 Key Points

* Policy lifecycle
* Exceptions registry
* Architecture diagrams
* Audit readiness

### 🎯 Interview Q&A

**Q. How do you handle business exceptions?**
**A:** Time-bound exception + approval + audit trail

---

## 🔹 SERVICES QUICK LIST (Must Know)

**Azure**

* Azure Policy
* Entra ID
* Azure DevOps
* Key Vault
* Monitor / Log Analytics

**AWS**

* IAM
* Organizations
* SCP / RCP
* AWS Config
* CloudTrail

**DevOps / IaC**

* Terraform
* Git
* CI/CD Pipelines
* Ansible (bonus)

---

## 🎓 CERTS THAT STRONGLY ALIGN

* Azure Administrator / Architect
* AWS Solutions Architect
* Terraform Associate (Nice to have)
* ITIL Foundation

---
