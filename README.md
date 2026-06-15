# Homelab Documentation

Professional documentation of a segmented home infrastructure environment built to simulate enterprise architecture and security testing scenarios.

This repository outlines the design, segmentation strategy, and security-focused lab environments used to develop and validate infrastructure hardening and attack simulation techniques.

All sensitive information, IP addressing, and identifying details are sanitized.

---

## High-Level Architecture

![Network Topology](Diagrams/Network-Topology.png)

---

## Architecture Overview

The lab environment is designed to model a layered enterprise network while maintaining controlled isolation for security experimentation.

Traffic flow begins at the ISP edge and passes through a Fortigate firewall before reaching a Cisco Layer 3 switch responsible for VLAN segmentation and inter-VLAN routing.

The Proxmox virtualization cluster is connected via VLAN 10 and hosts multiple virtual machines, including:

- Active Directory Domain Controller
- Windows client systems
- Kali Linux attacker workstation
- Additional infrastructure test systems

### AD Lab Isolation

The Active Directory security lab is further segmented using an internal Proxmox bridge (`vmbr1`) with no external routing. This allows controlled attack simulation without exposing the lab network to the internet.

Kali Linux is intentionally dual-homed to simulate realistic attacker positioning:

- Connected to the isolated AD lab bridge for lateral movement testing
- Connected to VLAN 10 for tool updates and controlled internet access

### Storage Segmentation

Storage infrastructure is isolated on VLAN 20, separating data traffic from compute and application workloads.

### Service Segmentation

Media services are placed on VLAN 30 to prevent unnecessary lateral exposure between entertainment workloads and core lab infrastructure.

This layered segmentation approach enforces routing boundaries and reduces attack surface across functional tiers.

---

## Security Controls Implemented

The lab environment incorporates layered security controls aligned with enterprise best practices:

- VLAN-based segmentation between compute, storage, and service tiers
- Hypervisor-level internal bridge isolation for AD testing
- Controlled dual-homing for attack simulation realism
- Firewall-enforced routing boundaries
- Separation of storage traffic from application workloads
- Principle of least privilege within AD test environment
- Tiered logical separation between infrastructure and service systems

This design enables safe simulation of offensive techniques while preserving infrastructure integrity.

---

## Security Lab Scope

The security lab environment supports:

- Active Directory deployment and configuration
- Kerberos attack simulation
- Credential attack testing
- Privilege escalation validation
- Defensive control evaluation
- Structured documentation of findings and mitigations

Documentation for lab builds and hardening analysis can be found in the `Security-Lab` directory.

---

## Infrastructure Scope

The broader infrastructure environment includes:

- Multi-node Proxmox virtualization cluster
- Layer 3 switching with VLAN segmentation
- Firewall boundary enforcement
- Segmented storage architecture
- Service tier isolation

Build documentation and infrastructure notes are maintained in the `Infrastructure` directory.

---

## Skills Demonstrated

This repository reflects hands-on experience with:

- Network segmentation and VLAN design
- Firewall boundary planning
- Hypervisor networking configuration (Proxmox)
- Active Directory lab deployment
- Kerberos attack path simulation
- Infrastructure hardening analysis
- Enterprise-style architecture modeling
- Technical documentation and structured reporting

---

## Purpose

This homelab environment exists to:

- Develop practical security engineering skills
- Validate segmentation and isolation strategies
- Simulate realistic attack paths in controlled conditions
- Translate offensive techniques into defensive improvements
- Strengthen infrastructure design and documentation discipline



