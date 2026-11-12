# POC-04 – BloodHound Attack Path Mapping

**ATT&CK:** T1482 – Domain Trust Discovery / Account Discovery
**Host:** Kali → Domain Controller
**Tools:** bloodhound-python (or SharpHound), BloodHound GUI, Neo4j

## Steps

### 1. Collect data from Kali
```bash
bloodhound-python -u domainuser -p 'Password123' -d lab.local -ns 192.168.56.10 -c All
```
This produces a set of JSON files describing users, groups, computers, sessions, and ACLs.

### 2. Start Neo4j and BloodHound
```bash
sudo neo4j start
bloodhound
```

### 3. Import and analyze
- Drag the JSON output into the BloodHound GUI.
- Use the built-in queries: **"Find Shortest Paths to Domain Admins"**, **"Find Principals with DCSync Rights"**.

## Observation
Record the discovered path(s) — e.g. `domainuser` → member of `HelpDesk` → `GenericAll` on `svc-sql` → `svc-sql` has Kerberoastable SPN → cracked password → session on `WIN10` → local admin → Domain Admin.

## Evidence
`![BloodHound shortest path graph](screenshots/poc04-bloodhound-path.png)`

## Impact
Visualizing the attack graph turns scattered misconfigurations (nested groups, stale ACLs, risky sessions) into a concrete, provable path to full domain compromise — exactly how a real attacker would plan lateral movement.

## Remediation
Regularly run BloodHound from the defensive side ("purple teaming"), remove unnecessary group nesting and ACL grants, and specifically audit for any non-Domain-Admin principal with a path to Domain Admin.
