# Decision Log – IR-003 RDP Brute Force

## Scenario
- Alert / Signal: Multiple failed RDP authentication attempts.
- Environment: Lab / simulated host (WIN-LAB-01).
- Date: [YYYY-MM-DD]

## Known facts (evidence)
- Fact 1:
  - Evidence: Repeated failed logon events via RDP (LogonType 10).
- Fact 2:
  - Evidence: Attempts originated from one or more external IP addresses.

## Unknowns (what I do NOT know yet)
- Unknown 1: Whether any authentication attempt eventually succeeded.
- Unknown 2: Whether valid credentials were compromised.
- Unknown 3: Whether similar activity occurred on other hosts.

## Hypotheses
H1:
- Why possible: Automated brute-force or credential-stuffing attack.
- Evidence for: High volume of failed RDP logons from external sources.
- Evidence against: No confirmed successful authentication observed.

H2:
- Why possible: Legitimate user repeatedly entering incorrect credentials.
- Evidence for: Possible misconfiguration or user error.
- Evidence against: External IP origin and repeated pattern.

## Quick checks (highest value first)
1) Check for any successful RDP logon following failures.
2) Identify targeted accounts and privilege level.
3) Review RDP exposure and network controls.

## Decision boundary
I will ESCALATE if:
- A successful RDP logon is detected after failed attempts.
- Privileged or sensitive accounts are targeted.
- Similar activity is observed across multiple systems.

I will CLOSE as benign if:
- No successful logons are identified.
- Activity stops after mitigation.
- RDP exposure is expected and controlled.

## Risk note (if I am wrong)
- Main risk: Unauthorized remote access via compromised credentials.
- Impact: Full system compromise and lateral movement.
- Why my boundary reduces risk: Focuses escalation on confirmation of access, not noise.

## Decision
- Action: Monitor closely and apply mitigation; no immediate escalation.
- Confidence: Medium.
- Reason: Clear brute-force pattern without evidence of successful access.

