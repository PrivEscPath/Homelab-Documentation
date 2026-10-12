# Virtualization

## Overview

The HuntWired homelab uses Proxmox Virtual Environment (VE) as its primary virtualization platform.

The environment supports infrastructure services, identity management, backup and recovery, security monitoring, network simulation, and offensive security testing.

The virtualization architecture integrates enterprise server hardware, Cisco networking, and TrueNAS-backed storage to provide a flexible environment for deploying and managing virtualized workloads.

## Hypervisor Infrastructure

### Proxmox VE

- **Platform:** Proxmox VE 9.1.7
- **Hardware:** Dell PowerEdge R620
- **Node Name:** cloud
- **Deployment:** Standalone, single-node hypervisor
- **Virtualization Technologies:** KVM/QEMU and Linux Containers (LXC)

Proxmox provides centralized administration of virtual machines, containers, storage resources, and virtual networking.

### Architecture Evolution

The environment was originally deployed as a multi-node Proxmox cluster using two Dell PowerEdge R620 servers.

The architecture was subsequently consolidated into a single-node deployment to reduce electrical consumption and improve resource utilization.

The second R620 was repurposed as a dedicated TrueNAS storage server.

This change separated compute and storage responsibilities while reducing the number of physical systems required to operate the environment.

The current architecture prioritizes resource efficiency, maintainability, and flexibility over hypervisor-level high availability.

## Virtual Machine Infrastructure

The Proxmox environment hosts several categories of virtual machines.

| Virtual Machine | Purpose |
|---|---|
| Veeam | Backup and recovery infrastructure |
| DC2 | Windows Server 2022 Active Directory and DNS |
| TestWorkstation | Windows testing and domain integration |
| Splunk | Security monitoring and log analysis |
| Veeam-Proxmox-Worker | Veeam-managed Proxmox backup processing |
| Kali Linux | Offensive security and penetration-testing environment |
| GNS3 | Enterprise network simulation and security testing |

Infrastructure services such as DC2 and Veeam operate continuously.

Resource-intensive lab environments, including GNS3 and security-testing systems, are powered on as needed.

### GNS3 Network Simulation

GNS3 runs as a virtual machine using Ubuntu Server 24.04.

The environment supports virtual network appliances and test systems, including:

- Cisco virtual switching
- Cisco ASAv firewall simulation
- Windows testing environments
- Additional network and security appliance experimentation

GNS3 operates within a dedicated network segment, VLAN 55, separating network simulation workloads from other infrastructure.

The environment is used to develop and validate networking configurations, security architectures, and troubleshooting procedures.

## Linux Containers

Proxmox also hosts lightweight LXC containers for infrastructure services.

| Container | Purpose |
|---|---|
| Cloudflared | Cloudflare Tunnel connectivity |
| Apache Guacamole | Browser-based remote access and administration |

Containers provide a lightweight alternative to full virtual machines for suitable infrastructure workloads.

## Storage Architecture

The virtualization environment integrates with a dedicated TrueNAS storage server.

Storage resources are separated by workload requirements and underlying storage technology.

### Storage Tiers

| Storage Resource | Purpose |
|---|---|
| vm-ssd | SSD-backed virtual machine storage |
| vm-hdd | HDD-backed virtual machine storage |
| ISO | Centralized operating system and installation media |
| Splunk-Thin | Dedicated Splunk storage |
| truenas-iscsi | TrueNAS iSCSI storage integration |
| local / local-lvm | Local Proxmox storage resources |

TrueNAS provides centralized storage resources through iSCSI and NFS.

Storage naming and organization were redesigned to clearly identify workload placement and distinguish SSD-backed from HDD-backed resources.

This separation simplifies administration, troubleshooting, and storage capacity planning.

## Network Integration

Proxmox integrates with the Cisco Catalyst 3850 network core.

The network architecture includes:

- LACP-based connectivity
- VLAN-aware virtual networking
- Dedicated infrastructure and management networks
- Segmented virtual machine environments
- Separate networking for security testing and simulation

VLAN 50 provides primary Proxmox infrastructure connectivity.

VLAN 51 supports a Windows testing environment, while VLANs 52–54 are reserved for future lab expansion.

VLAN 55 is dedicated to GNS3.

Inter-VLAN routing and network access restrictions are enforced by the Cisco Catalyst 3850.

## Virtualization Management

The environment supports:

- Centralized VM and container administration
- VM templates and cloning
- Virtual machine snapshots
- Resource allocation and monitoring
- Integration with external storage
- VM-level backup and recovery through Veeam

The environment balances continuously running infrastructure services with on-demand lab workloads to manage available compute, memory, storage, and electrical resources.

## Planned Improvements

- Continue optimizing virtual machine storage placement.
- Expand GNS3 network simulation capabilities.
- Improve infrastructure automation and configuration management.
- Expand monitoring and performance analysis.
- Evaluate future compute and storage hardware upgrades.

## Future Technical Documentation

Detailed Proxmox configurations, storage integration, network configuration, GNS3 deployment, and troubleshooting procedures will be maintained in dedicated technical repositories.
