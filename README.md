# Active Directory Red Team Home Lab

![Type](https://img.shields.io/badge/Type-Red%20Team-blue) ![Target](https://img.shields.io/badge/Target-Active%20Directory-green) ![Tools](https://img.shields.io/badge/Tools-Kali%20%7C%20Impacket%20%7C%20BloodHound-orange) ![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red)

A home-lab Active Directory red team engagement, built to practice common AD attack techniques end-to-end in a safe, fully self-owned environment, and documented the way a real internal/red-team engagement would be.

> ⚠️ **Scope & Ethics:** Every system in this lab (domain controller, Windows 10 workstation, Kali attack box) is self-hosted on personally owned/virtualized infrastructure. Nothing here was run against a third-party network. Never use these techniques outside a lab or an environment you are explicitly authorized to test.

## About the Author
VAPT Analyst with experience across bug bounty (HackerOne), banking (MCB Bank), and healthcare technology (CareCloud). Primary focus is web/API pentesting; this lab is self-driven practice extending into internal network and Active Directory red teaming.

## Lab Environment

| Role | OS | Purpose |
|---|---|---|
| Domain Controller | Windows Server (AD DS installed) | Root of the domain, holds `NTDS.dit` |
| Workstation | Windows 10, domain-joined | Simulates a regular domain user's machine |
| Attack box | Kali Linux | Red team operator machine |

```
                 ┌───────────────────────┐
                 │   Windows Server       │
                 │   (Domain Controller)  │
                 └──────────┬─────────────┘
                            │ Domain: lab.local
                 ┌──────────┴─────────────┐
                 │                        │
        ┌────────▼────────┐     ┌─────────▼────────┐
        │  Windows 10 VM   │     │   Kali Linux VM   │
        │ (domain-joined)  │     │  (attacker box)    │
        └──────────────────┘     └────────────────────┘
```

## Key Features
- End-to-end AD attack chain: recon → initial foothold → credential access → lateral movement → domain privilege escalation
- Every technique mapped to **MITRE ATT&CK**
- Proof-of-concept evidence for each stage (commands, tool output, screenshots)
- Written the way a real red-team/internal pentest report is structured

## Technologies
`Kali Linux` · `Impacket` · `BloodHound` · `CrackMapExec / NetExec` · `Mimikatz` · `Responder` · `PowerView` · `MITRE ATT&CK`

## Repository Structure
```
.
├── README.md
├── docs/
│   ├── 01-lab-setup.md
│   ├── 02-scope-and-rules.md
│   ├── 03-attack-chain-methodology.md
│   └── 04-findings-summary.md
├── pocs/
│   ├── POC-01-network-recon-enumeration.md
│   ├── POC-02-llmnr-nbtns-poisoning-responder.md
│   ├── POC-03-kerberoasting.md
│   ├── POC-04-bloodhound-attack-path-mapping.md
│   ├── POC-05-lateral-movement-psexec-wmiexec.md
│   └── POC-06-domain-admin-dcsync.md
├── tools/
│   └── tool-notes.md
└── reports/
    └── RedTeam_Report_Template.md
```
> Screenshots and raw tool output go in `pocs/screenshots/`, referenced from each POC file.

## Attack Chain (Summary)
1. **Recon** – network/host discovery, SMB/LDAP enumeration (`nmap`, `enum4linux-ng`, `netexec`)
2. **Initial credential access** – LLMNR/NBT-NS poisoning with Responder, hash capture
3. **Kerberoasting** – request service tickets for SPN accounts, crack offline
4. **Attack path mapping** – BloodHound ingestion and analysis
5. **Lateral movement** – authenticate to the Windows 10 host using recovered creds (PsExec/WMIexec)
6. **Domain compromise** – privilege escalation to Domain Admin, DCSync to dump `NTDS.dit` hashes

## Findings Overview
See [`docs/04-findings-summary.md`](docs/04-findings-summary.md).

## Disclaimer
For educational use only, inside a fully self-owned, isolated home lab. The author is not responsible for misuse of these techniques against systems without explicit authorization.
