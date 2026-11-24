# Standard Operating Procedure (SOP): Joiner-Mover-Leaver (JML) Lifecycle

## 1. Purpose & Scope
This runbook documents the operational procedures used in Microsoft Entra ID (Free Tier) to manage user identities across the **Joiner**, **Mover**, and **Leaver** lifecycle stages using test persona **David Miller** (`david.miller@stueduswapnilgmail.onmicrosoft.com`).

---

## 2. Phase 1: Joiner (Onboarding & Baseline Provisioning)
**Objective:** Provision a new identity with role-appropriate attributes and baseline department access.

1. **Create User Identity:**
   - Navigate to **Microsoft Entra ID > Users > New user > Create new user**.
   - Configure `Display name: David Miller` and `User principal name: david.miller@stueduswapnilgmail.onmicrosoft.com`.
2. **Assign Organizational Attributes:**
   - Open **David Miller > Properties > Edit properties** (or **Job Information**).
   - Set `Job title: Financial Analyst` and `Department: Finance`.
3. **Grant Baseline RBAC Access:**
   - Assign user to the Finance security group (`SG-Finance-Access`).
   - Assign user to the enterprise application (`Finance-App-Prod`) under **Enterprise applications > Finance-App-Prod > Users and groups**.

---

## 3. Phase 2: Mover (Role Transition Simulation)
**Objective:** Update user attributes following an internal departmental transfer and observe entitlement state.

1. **Update HR / Job Attributes:**
   - Open **David Miller > Properties > Job Information**.
   - Modify `Job title` from `Financial Analyst` to `HR Coordinator` and `Department` from `Finance` to `HR`.
2. **Observe Access Drift State:**
   - Verify that without dynamic group rules or automated lifecycle workflows, legacy Finance group and application assignments remain active after the department change.

---

## 4. Phase 3: Leaver (Offboarding & Access Revocation)
**Objective:** Immediately block authentication, terminate active sessions, and verify zero lingering access.

1. **Confirm Zero Entitlements:**
   - Verify **Groups** displays `Not a member of any groups` and **Applications** displays `No application assignments found`.
2. **Disable Account & Revoke Sessions:**
   - Open **David Miller > Properties > Settings** and set `Account enabled` to `No` (`Disable account`).
   - Select **Revoke sessions** on the user's **Overview** blade to invalidate active refresh tokens.
3. **Audit Verification:**
   - Open **David Miller > Audit logs** and verify `Disable account` and `Update user` events show `Status: Success`.
