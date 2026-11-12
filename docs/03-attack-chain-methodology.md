# Attack Chain Methodology

Mapped loosely to MITRE ATT&CK tactics.

## 1. Reconnaissance (TA0043) & Discovery (TA0007)
- Host discovery: `nmap -sn 192.168.56.0/24`
- Service enumeration: `nmap -p- -sC -sV 192.168.56.10`
- SMB/LDAP enum: `enum4linux-ng`, `netexec smb`, `ldapsearch`

## 2. Credential Access (TA0006) – Network Poisoning
- Run **Responder** on Kali to capture NTLMv2 hashes from broadcast name-resolution requests (LLMNR/NBT-NS) triggered from the Windows 10 host.
- Crack captured hashes offline with `hashcat`.

## 3. Credential Access – Kerberoasting
- Enumerate SPNs with `impacket-GetUserSPNs`.
- Request and crack service ticket hashes offline.

## 4. Discovery – Attack Path Mapping
- Collect AD data with `bloodhound-python` / SharpHound.
- Analyze in BloodHound GUI for shortest paths to Domain Admin.

## 5. Lateral Movement (TA0008)
- Use recovered/cracked credentials to authenticate to the Windows 10 host via `impacket-wmiexec` / `impacket-psexec`.

## 6. Privilege Escalation (TA0004) & Domain Compromise
- Exploit a discovered misconfiguration (e.g. unconstrained delegation, nested group membership) to reach Domain Admin.
- Perform a **DCSync** attack with `impacket-secretsdump` to extract domain hashes.

## 7. Reporting
- Document each stage with commands, output, and screenshots.
- Map each finding to a remediation and an ATT&CK technique ID.
