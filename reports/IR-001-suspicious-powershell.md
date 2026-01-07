# Incident Report: IR-001 – Suspicious PowerShell (Encoded Command)

## 1) Executive summary (6 lines max)

- What happened: PowerShell executed with an encoded command and a hidden window.
- How we detected it: Process creation logs (Sysmon / Security) showed suspicious execution flags.
- What we believe: Possible script-based malware execution or initial staging activity.
- What we do next: Validate parent process, decode the command, and check network and persistence activity.
- Confidence: Medium (strong suspicious indicators, but limited full context).
- Main risk: Remote payload download/execution and potential lateral movement if the host is compromised.

---

## 2) Scope

- Assets / host: WIN-LAB-01 (lab / simulated host)
- Accounts: Local user account (lab user)
- Time window: Approximately 10–15 minutes around the suspicious execution

---

## 3) Timeline (key events)

- T0: User context active on the host (baseline).
- T1: `powershell.exe` started with `-EncodedCommand` and hidden window parameters.
- T2: Possible outbound network activity shortly after execution (requires confirmation).
- T3: Initial triage checks initiated (log review and basic host analysis).

---

## 4) Evidence (what I can prove)

- Process execution indicates high-risk PowerShell usage (encoded command and hidden window).

**Event:** Process Create (example)  
**Image:** C:\Windows\System32\WindowsPowerShell\v1.0 `powershell.exe`  
**CommandLine:** powershell -NoProfile `-WindowStyle Hidden` -ExecutionPolicy `Bypass` `-EncodedCommand`[base64]  
**ParentImage:** [unknown / not captured in this snapshot]  
**User:** [lab-user]

**Notes:**
- Flags such as `-EncodedCommand`, `-WindowStyle Hidden`, and `Bypass` are commonly observed in malicious scripting activity.
- The parent process is critical context (for example: Office application, wscript, browser, explorer).

---

## 5) Analysis (evidence-first)

**Most likely explanation:**

Script execution intended to hide activity and possibly download or run a malicious payload.

**Alternative explanation:**

Legitimate administrative automation or a security tool using encoded commands, but poorly documented.

**What I cannot confirm:**

- Whether the encoded content was malicious without decoding it.
- Whether any payload was successfully downloaded or executed.
- Whether persistence mechanisms or credential access occurred.

---

## 6) MITRE ATT&CK mapping (1–3 items)

- T1059.001 – Command and Scripting Interpreter: PowerShell  
  - PowerShell was used for command execution.

- T1027 – Obfuscated/Compressed Information  
  - The encoded command represents obfuscation.

- T1105 – Ingress Tool Transfer (optional)  
  - Applicable only if evidence confirms a remote payload download.

---

## 7) Response actions (risk-aware)

**Containment (proposed or executed):**

- In a production environment, isolate the host from the network while keeping it powered on to preserve evidence.
- Block known suspicious domains or IP addresses if identified.

**Eradication (proposed or executed):**

- Remove confirmed malicious artifacts only after evidence collection.
- Reset affected credentials if compromise is confirmed.

**Recovery (proposed or executed):**

- Reconnect the host to the network after clean verification.
- Add detections for similar PowerShell execution patterns.

---

## 8) Decision boundaries (what triggers escalation)

**Escalate if:**

- Encoded PowerShell with a hidden window and a suspicious parent process (Office, wscript, rundll32).
- Any outbound connection to an unknown external host shortly after execution.
- Creation of scheduled tasks, services, or registry run keys after T1.

**Close if:**

- The decoded command is benign, documented, and matches an approved IT or security script.
- The script is signed or approved and logs show no suspicious network or persistence activity.

---

## 9) Lessons learned

**Detection improvement:**

- Alert on PowerShell executions combining `-EncodedCommand`, hidden window, and execution policy bypass.

**Logging improvement:**

- Ensure parent process information is captured (Sysmon configuration or Security Event ID 4688 with full command line).
- Enable PowerShell Script Block Logging in the lab to improve evidence quality.

**Playbook improvement:**

- Standardize a flow for PowerShell alerts: decode command → validate intent → correlate network activity → check for persistence.
