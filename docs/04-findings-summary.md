# Findings Summary

> Fill in with what you actually achieved in your lab run. Typical findings for a lab configured this way are listed below as a starting template — confirm each against your own session.

| ID | Title | ATT&CK Technique | Severity | Status |
|---|---|---|---|---|
| F-01 | LLMNR/NBT-NS enabled, allowing credential capture via Responder | T1557.001 | High | Open |
| F-02 | Service account with SPN crackable via Kerberoasting (weak password) | T1558.003 | High | Open |
| F-03 | Attack path to Domain Admin discoverable via BloodHound (nested groups) | T1482 | High | Open |
| F-04 | Lateral movement possible with captured/cracked credentials (no SMB signing enforced) | T1021.002 | Medium | Open |
| F-05 | DCSync possible from compromised privileged account | T1003.006 | Critical | Open |

## Remediation Themes
- Disable LLMNR and NBT-NS via Group Policy.
- Enforce strong, long passphrases for all service accounts; prefer Group Managed Service Accounts (gMSA).
- Enforce SMB signing domain-wide.
- Regularly audit AD with BloodHound from the defensive side; remove unnecessary nested group paths to Domain Admin.
- Restrict and monitor accounts with DCSync-equivalent rights (`Replicating Directory Changes` / `Replicating Directory Changes All`).
