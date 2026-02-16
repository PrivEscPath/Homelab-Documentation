# Active Directory Lab Build

## Objective

Build a segmented Active Directory lab environment for security testing and attack path analysis.

---

## Lab Architecture

### Components
- 1x Windows Server (Domain Controller)
- 2x Windows 10/11 Domain-Joined Client
- 1x Kali Linux Attacker Machine
- Internal isolated VLAN

### Virtualization Platform
- Proxmox VE
- Internal virtual bridge (no internet exposure)
- Static IP assignments within lab subnet

---

## Network Design

- Lab subnet: 10.10.100.0/24
- Domain Controller: 10.10.100.100
- Client: 10.10.100.1
- Client: 10.10.100.2
- Kali: 10.10.100.30
- Gateway: None (isolated)

All IP addresses are anonymized for documentation purposes.

---

## Domain Setup

- Forest/domain created
- DNS configured on DC
- Clients joined to domain
- Test users created for attack simulation

---

## Security Testing Goals

- User enumeration
- Kerberos-based attacks
- Credential harvesting techniques
- Privilege escalation validation

---

## Lessons Learned

- Importance of proper DNS configuration
- Time synchronization issues impacting Kerberos
- Benefits of network isolation during testing
