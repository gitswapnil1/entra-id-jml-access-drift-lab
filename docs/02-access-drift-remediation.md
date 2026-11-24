# Access Drift Analysis & Manual Entitlement Remediation

## 1. Problem Statement: What is Access Drift?
**Access drift (privilege creep)** occurs when a user changes roles or departments inside an organization and accumulates new permissions without having legacy entitlements revoked. In this lab, when **David Miller** transferred from **Finance** (`Financial Analyst`) to **HR** (`HR Coordinator`), retaining access to `Finance-App-Prod` and `SG-Finance-Access` violated the **Principle of Least Privilege (PoLP)** and created a **Segregation of Duties (SoD)** risk.

---

## 2. Manual Access Review & Remediation Workflow
Because automated Entra ID Access Reviews require a **Microsoft Entra ID P2** license, a manual entitlement audit was executed:

| Audit Step | Target Blade | Finding | Remediation Action | Verification Result |
| :--- | :--- | :--- | :--- | :--- |
| **1. Role vs. Entitlement Check** | `David Miller > Properties` | Department updated to `HR`, but Finance access remained active. | Flagged Finance entitlements as stale/unauthorized. | Confirmed mismatch between HR role and Finance permissions. |
| **2. Enterprise App Audit** | `David Miller > Applications` | Direct assignment to `Finance-App-Prod` present. | Removed user assignment from `Finance-App-Prod`. | `No application assignments found.` |
| **3. Security Group Audit** | `David Miller > Groups` | Membership in `SG-Finance-Access` present. | Removed user from security group. | `Not a member of any groups.` |
| **4. Audit Log Verification** | `David Miller > Audit logs` | Needed immutable proof of revocation. | Reviewed directory logs (`Remove member from group`). | Confirmed `Status: Success` at `17:28:02`. |

---

## 3. Enterprise Automation Roadmap (Preventing Drift at Scale)
To eliminate manual review overhead in production tenants licensed with **Microsoft Entra ID P1/P2**:
- **Attribute-Based Dynamic Groups (Entra ID P1):** Replace static group assignments with dynamic membership rules such as `(user.department -eq "Finance")` so department updates automatically revoke old group access.
- **Automated Access Reviews (Entra ID P2):** Configure recurring quarterly reviews on `Finance-App-Prod` requiring managers or resource owners to recertify access, with auto-revocation enabled for non-responders.
- **Lifecycle Workflows (Entra ID Governance):** Trigger pre-built **Mover** and **Leaver** workflows tied to HR attribute changes to revoke sessions, remove group memberships, and disable accounts automatically.
