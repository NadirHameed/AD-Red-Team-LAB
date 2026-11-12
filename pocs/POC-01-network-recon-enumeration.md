# POC-01 – Network Recon & Enumeration

**ATT&CK:** Discovery (TA0007), Active Scanning (T1595)
**Host:** Kali → Domain Controller / Windows 10
**Tools:** nmap, enum4linux-ng, netexec

## Steps

### 1. Host discovery
```bash
nmap -sn 192.168.56.0/24
```

### 2. Full port/service scan of the DC
```bash
nmap -p- -sC -sV -T4 192.168.56.10 -oN dc-scan.txt
```
Expect to see: `53` (DNS), `88` (Kerberos), `135/445` (RPC/SMB), `389/636` (LDAP/LDAPS), `3268/3269` (Global Catalog).

### 3. SMB/LDAP enumeration
```bash
enum4linux-ng -A 192.168.56.10
netexec smb 192.168.56.10 -u '' -p '' --shares
ldapsearch -x -H ldap://192.168.56.10 -b "dc=lab,dc=local"
```

## Observation
Record the domain name, DC hostname, discovered shares, and any users/groups visible via anonymous/null-session enumeration.

## Evidence
`![nmap full scan output](screenshots/poc01-nmap-scan.png)`

## Impact
Confirms the domain name, exposed services, and whether null-session/anonymous enumeration is possible — the foundation for every later step.

## Remediation
Restrict anonymous SMB/LDAP enumeration, disable unused services, and segment the network so a compromised host cannot freely port-scan the DC.
