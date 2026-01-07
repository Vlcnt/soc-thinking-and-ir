# Decision Log – IR-001 Suspicious PowerShell

## Scenario
- Alert / Signal: Suspicious PowerShell execution with encoded command and hidden window.
- Environment: Lab / simulated host (WIN-LAB-01).
- Date: [YYYY-MM-DD]

## Known facts (evidence)
- Fact 1:
  - Evidence: Process creation shows `powershell.exe` with `-EncodedCommand` and hidden window.
- Fact 2:
  - Evidence: Execution policy bypass observed during PowerShell launch.

## Unknowns (what I do NOT know yet)
- Unknown 1: Actual content of the encoded command.
- Unknown 2: Whether outbound network activity resulted in payload download.

## Hypotheses
H1:
- Why possible: Encoded PowerShell and hidden window are common in malicious scripting.
- Evidence for: Execution flags and obfuscation indicators.
- Evidence against: No confirmed malicious payload or persistence observed.

H2:
- Why possible: Legitimate administrative automation using encoded commands.
- Evidence for: Possible use in IT or security scripts.
- Evidence against: Lack of documentation and use of hidden window.

## Quick checks (highest value first)
1) Decode the PowerShell command to understand intent.
2) Identify and validate the parent process.
3) Correlate with network logs around execution time.

## Decision boundary
I will ESCALATE if:
- Decoded command shows malicious intent or remote payload download.
- Suspicious parent process (Office, wscript, rundll32) is confirmed.
- Persistence artifacts are created after execution.

I will CLOSE as benign if:
- Decoded command is benign and documented.
- Script is approved/signed and no network or persistence activity is found.

## Risk note (if I am wrong)
- Main risk: Undetected malware execution leading to further compromise.
- Impact: Possible lateral movement or data exfiltration.
- Why my boundary reduces risk: Escalation triggers focus on behaviors with highest impact.

## Decision
- Action: Monitor and investigate further; no immediate closure.
- Confidence: Medium.
- Reason: Strong suspicious indicators with incomplete confirmation.

