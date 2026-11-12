# Scope & Rules of Engagement

| Item | Detail |
|---|---|
| Target domain | `lab.local` (self-hosted, isolated internal network) |
| Domain Controller | Windows Server, `192.168.56.10` |
| Workstation | Windows 10, `192.168.56.20`, domain-joined |
| Attacker | Kali Linux, `192.168.56.30` |
| Authorization | Entirely self-owned home lab |
| In scope | All three lab VMs, on the isolated internal/host-only network |
| Out of scope | The physical host OS, home network/router, any system outside the lab's internal network |
| Standards | MITRE ATT&CK, internal AD pentest methodology |

## Rules
- All VMs stay on an isolated internal network with no route to the internet or the home LAN.
- No techniques are run against anything outside this lab.
- Snapshot all VMs before destructive tests (e.g. before DCSync, before testing lockout policies) so the lab can be reset.
