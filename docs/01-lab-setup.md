# Lab Setup

## Virtualization
- Hypervisor: VMware Workstation / VirtualBox (either works)
- Networking: all three VMs on an **internal/host-only network**, isolated from the internet and from the home LAN, to avoid any accidental traffic leaving the lab.

## VM 1 – Domain Controller (Windows Server)
1. Install Windows Server (Evaluation ISO from Microsoft is fine for a lab).
2. Set a static IP, e.g. `192.168.56.10`.
3. **Server Manager → Add Roles and Features → Active Directory Domain Services** → promote to a new forest, e.g. domain `lab.local`.
4. Create a few test OUs, groups, and users (including at least one **service account with an SPN** set, to support Kerberoasting practice later):
```powershell
setspn -A lab.local/sqlsvc svc-sql
```
5. Create a "Domain Admins" style misconfiguration on purpose for later practice, e.g. nested groups, or a user with unconstrained delegation.

## VM 2 – Windows 10 (domain-joined workstation)
1. Install Windows 10, set static IP `192.168.56.20`.
2. **System → About → Domain or workgroup** → join `lab.local` using domain admin creds.
3. Log in as a domain user at least once so the machine caches a profile.
4. Optionally disable Windows Defender real-time protection in the lab VM only, to reduce noise while learning (never do this on a real network).

## VM 3 – Kali Linux (attacker)
1. Install Kali, static IP `192.168.56.30`, same internal network.
2. Update tools: `sudo apt update && sudo apt full-upgrade -y`
3. Install/verify: `impacket-scripts`, `responder`, `bloodhound`, `neo4j`, `crackmapexec` or `netexec`, `hashcat`, `seclists`.

## Connectivity check
```bash
ping 192.168.56.10   # DC
ping 192.168.56.20   # Win10
```
