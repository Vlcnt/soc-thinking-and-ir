# Incident Report: IR-003 – RDP Brute Force / Suspicious Authentication Attempts

## 1) Executive summary (6 lines max)

- What happened: Multiple failed RDP logon attempts were observed against a Windows host.
- How we detected it: Windows Security logs showed repeated authentication failures from external IP addresses.
- What we believe: Likely brute-force or credential-stuffing attempts targeting RDP access.
- What we do next: Assess success of any logon, validate exposed services, and evaluate need for escalation.
- Confidence: Medium (clear brute-force pattern, no confirmed successful access).
- Main risk: Unauthorized remote access leading to full system compromise.

---

## 2) Scope

- Assets / host: WIN-LAB-01 (lab / simulated host)
- Accounts: One or more local or domain user accounts
- Time window: Approximately 20–30 minutes of repeated authentication attempts

---

## 3) Timeline (key events)

- T0: Baseline state with normal authentication activity.
- T1: Burst of failed RDP logon attempts detected.
- T2: Attempts continue from one or more external IP addresses.
- T3: Triage initiated to determine whether any logon succeeded.

---

## 4) Evidence (what I can prove)

- Windows Security logs indicate repeated failed RDP authentication attempts.

**Event:** Failed Logon  
**Event ID:** 4625  
**LogonType:** 10 (RemoteInteractive / RDP)  
**TargetAccount:** [target-user]  
**SourceIP:** [external-ip]

**Notes:**
- LogonType 10 is associated with RDP access.
- Repeated failures from external IPs commonly indicate brute-force activity.
- Failed attempts alone do not confirm compromise, but increase exposure risk.

---

## 5) Analysis (evidence-first)

**Most likely explanation:**

Automated brute-force or credential-stuffing attempts against exposed RDP service.

**Alternative explanation:**

Misconfigured system or legitimate user repeatedly entering incorrect credentials.

**What I cannot confirm:**

- Whether any authentication attempt eventually succeeded.
- Whether valid credentials were obtained.
- Whether the same activity is occurring on other hosts.

---

## 6) MITRE ATT&CK mapping (1–3 items)

- T1110 – Brute Force  
  - Repeated authentication attempts against the same service.

- T1021.001 – Remote Services: RDP  
  - RDP is the targeted access mechanism.

---

## 7) Response actions (risk-aware)

**Containment (proposed or executed):**

- Restrict or temporarily disable RDP exposure if not required.
- Block or rate-limit offending IP addresses.

**Eradication (proposed or executed):**

- Enforce strong password and lockout policies.
- Review and reset credentials if compromise is suspected.

**Recovery (proposed or executed):**

- Confirm no successful logons occurred.
- Monitor for renewed authentication attempts after mitigation.

---

## 8) Decision boundaries (what triggers escalation)

**Escalate if:**

- Any successful RDP logon (Event ID 4624, LogonType 10) follows failed attempts.
- Attempts target privileged or sensitive accounts.
- Similar activity is observed across multiple hosts.

**Close if:**

- No successful logons are detected.
- Activity stops after blocking or mitigation.
- RDP exposure is intentional and properly controlled.

---

## 9) Lessons learned

**Detection improvement:**

- Alert on repeated failed RDP logons from the same source IP.

**Logging improvement:**

- Ensure Event IDs 4625 and 4624 are retained and correlated.
- Capture source IP and account context for authentication events.

**Playbook improvement:**

- Standardize triage for RDP brute-force alerts:
  identify pattern → check for success → assess exposure → decide escalation.

