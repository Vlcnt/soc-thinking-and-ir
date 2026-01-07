# Incident Report: IR-002 – New Local Administrator Account

## 1) Executive summary (6 lines max)

- What happened: A new local administrator account was created on a Windows host.
- How we detected it: Windows Security logs showed local account creation and addition to the Administrators group.
- What we believe: Possible unauthorized privilege escalation or persistence setup.
- What we do next: Identify the creator, validate authorization, and assess follow-on use.
- Confidence: Medium (strong privilege signal, limited surrounding context).
- Main risk: Full system control via elevated privileges, enabling persistence and lateral movement.

---

## 2) Scope

- Assets / host: WIN-LAB-01 (lab / simulated host)
- Accounts: Newly created local account; local Administrators group
- Time window: Approximately 15 minutes around account creation and group change

---

## 3) Timeline (key events)

- T0: Baseline state with no recent administrative changes.
- T1: Local user account created.
- T2: New account added to the local Administrators group.
- T3: Initial triage and context validation started.

---

## 4) Evidence (what I can prove)

- Windows Security events indicate creation of a local account and assignment of administrative privileges.

**Event:** User Account Created  
**Event ID:** 4720  
**TargetAccount:** [new-user]  
**SubjectAccount:** [creator-account]

**Event:** User Added to Local Administrators Group  
**Event ID:** 4732  
**Group:** Administrators  
**Member:** [new-user]

**Notes:**
- Membership in the local Administrators group grants full control over the system.
- This signal alone is high-impact, even without additional malicious activity.

---

## 5) Analysis (evidence-first)

**Most likely explanation:**

Unauthorized creation of a privileged account to gain or maintain administrative access.

**Alternative explanation:**

Legitimate administrative activity (IT support, system provisioning, or maintenance).

**What I cannot confirm:**

- Whether the account creation was approved without change documentation.
- Whether the new account was used for logon or further actions.
- Whether this change is isolated or part of a broader attack chain.

---

## 6) MITRE ATT&CK mapping (1–3 items)

- T1136 – Create Account  
  - A new local account was created on the system.

- T1098 – Account Manipulation  
  - The account was added to the local Administrators group.

---

## 7) Response actions (risk-aware)

**Containment (proposed or executed):**

- Disable the newly created account pending authorization checks.
- Remove the account from the Administrators group if unauthorized.

**Eradication (proposed or executed):**

- Delete unauthorized accounts after evidence preservation.
- Reset credentials and review admin access paths if misuse is confirmed.

**Recovery (proposed or executed):**

- Restore least-privilege configuration.
- Review recent logons and actions performed by the new account.

---

## 8) Decision boundaries (what triggers escalation)

**Escalate if:**

- The creator account is not authorized to perform admin changes.
- There is no documented approval for the new administrator account.
- The new account is used for logon, remote access, or further privilege changes.

**Close if:**

- Account creation is approved and documented.
- The account is temporary, monitored, and no suspicious follow-on activity is observed.

---

## 9) Lessons learned

**Detection improvement:**

- Alert on local account creation and additions to the Administrators group.

**Logging improvement:**

- Ensure Security Event IDs 4720 and 4732 are retained and monitored.
- Correlate with logon events (e.g., 4624) to establish usage context.

**Playbook improvement:**

- Standardize a verification flow for new admin accounts:
  validate request → confirm executor → assess follow-on activity.

