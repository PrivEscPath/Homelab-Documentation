# Active Directory

## Overview

The HuntWired homelab includes a Microsoft Active Directory environment built on Windows Server 2022.

The environment was deployed to develop hands-on experience with Windows domain administration, centralized authentication, DNS, Group Policy, and identity infrastructure.

Originally deployed with two domain controllers, the environment has since transitioned to a virtualized primary domain controller hosted on Proxmox.

Active Directory also serves as a platform for future identity management, security testing, and Linux integration projects.

## Domain Architecture

### DC2 — Active Domain Controller

- **Operating System:** Windows Server 2022
- **Platform:** Virtual machine hosted on Proxmox
- **Operational Status:** Online
- **Primary Functions:**
  - Active Directory Domain Services (AD DS)
  - DNS
  - Domain authentication
  - Group Policy infrastructure
  - FSMO role ownership

DC2 operates continuously and provides DNS services within the homelab.

### DC1 — Offline Domain Controller

- **Operating System:** Windows Server 2022
- **Hardware:** HP ProLiant DL360 G7
- **Platform:** Physical server
- **Operational Status:** Powered down

DC1 originally participated in the Active Directory environment alongside DC2.

All five FSMO roles were transferred to DC2 before DC1 was powered down.

DC1 remains part of the documented infrastructure but is not currently providing domain services.

## Implemented Features

### Active Directory Domain Services

- Windows Server 2022 domain environment
- Physical and virtual domain controller deployment
- Active Directory-integrated DNS
- Domain authentication services
- Windows workstation domain-join testing

### FSMO Role Migration

The five Flexible Single Master Operations (FSMO) roles were transferred from DC1 to DC2.

This migration allowed the virtualized domain controller to assume responsibility for domain-wide operations before the physical controller was taken offline.

### Group Policy

Group Policy has been used for basic domain administration and configuration testing, including:

- Domain-joined workstation configuration
- Desktop wallpaper deployment
- Initial policy management and testing

Additional security-focused Group Policy development is planned.

## Current Environment

The Active Directory environment currently supports infrastructure services and controlled testing.

DC2 remains online for DNS and domain services.

Household Windows devices are not currently domain-joined. Domain-joined workstation testing has been performed using virtual machines.

The environment is maintained primarily for infrastructure learning, identity administration, and future security testing.

## Planned Improvements

- Evaluate joining an existing Linux desktop to Active Directory.
- Explore Linux authentication using SSSD and Kerberos.
- Expand Group Policy security configurations.
- Develop additional domain-joined test workstations.
- Explore Active Directory security auditing and attack-path analysis.
- Review domain controller redundancy and recovery procedures.

## Future Technical Documentation

Detailed implementation guides, FSMO migration procedures, Group Policy configurations, Linux domain integration, and security testing will be maintained in a dedicated Active Directory technical repository.
