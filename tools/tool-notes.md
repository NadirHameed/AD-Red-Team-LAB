# Tool Notes

## Kali setup checklist
```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y responder bloodhound neo4j hashcat seclists
pip3 install impacket bloodhound
```

## Quick reference

| Tool | Use |
|---|---|
| `nmap` | Host/service discovery |
| `enum4linux-ng` | SMB/LDAP null-session enumeration |
| `netexec` (formerly CrackMapExec) | Credential spraying, SMB checks, "Pwn3d!" admin-access detection |
| `responder` | LLMNR/NBT-NS poisoning, hash capture |
| `impacket-GetUserSPNs` | Kerberoasting |
| `bloodhound-python` / SharpHound | AD data collection for BloodHound |
| `impacket-wmiexec` / `impacket-psexec` | Remote command execution / lateral movement |
| `impacket-secretsdump` | DCSync, local SAM dumping |
| `hashcat` | Offline password cracking |

## Useful hashcat modes
| Mode | Hash type |
|---|---|
| 1000 | NTLM |
| 5600 | NetNTLMv2 |
| 13100 | Kerberos TGS-REP (Kerberoasting) |
| 18200 | Kerberos AS-REP (AS-REP Roasting) |

## Snapshot discipline
Snapshot all three VMs before any destructive or state-changing test (DCSync, account lockout tests, password changes) so the lab can be reset to a known-good state.
