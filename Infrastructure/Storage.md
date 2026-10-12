# Storage Infrastructure

## Overview

The HuntWired homelab uses a dedicated TrueNAS SCALE server to provide centralized storage for virtual machines, infrastructure backups, installation media, personal file storage, and archival data.

The storage architecture integrates ZFS storage pools, enterprise server hardware, and multiple storage protocols to support different workload requirements.

Storage resources are organized according to performance, redundancy, capacity, and intended use.

The environment provides practical experience with enterprise storage administration, storage provisioning, network-based storage protocols, and infrastructure capacity management.

## Storage Hardware

### TrueNAS SCALE

| Component | Configuration |
|---|---|
| Storage Platform | TrueNAS SCALE |
| Server Hardware | Dell PowerEdge R620 |
| Expansion Hardware | Dell SC200 Expansion Shelves |
| Filesystem | OpenZFS |
| Storage Protocols | iSCSI, NFS, SMB |
| Network Infrastructure | Cisco Catalyst 3850 |

The TrueNAS server was previously part of a multi-node Proxmox virtualization cluster.

Following infrastructure consolidation, the second Dell R620 was repurposed as a dedicated storage server.

This architecture separates virtualization compute resources from centralized storage while reducing electrical consumption and maximizing the usefulness of existing hardware.

## ZFS Storage Pools

Three ZFS storage pools provide different levels of capacity, redundancy, and performance.

| Pool | Usable Capacity | Configuration | Primary Purpose |
|---|---|---|---|
| hdd-main | 10.6 TiB | 4 × 5-disk RAIDZ2 | Primary infrastructure and file storage |
| hdd-backup | 2.55 TiB | 4-disk RAIDZ1 | Backup repositories |
| ssd-scratch | 178.28 GiB | Single SSD | SSD-backed virtual machine storage |

### Primary HDD Pool — hdd-main

The primary storage pool consists of four RAIDZ2 vdevs, each containing five disks.

Two additional disks are configured as hot spares.

This pool provides:

- Redundant HDD-backed storage
- Proxmox virtual machine disk storage
- NFS installation media repository
- Personal and archival SMB shares
- Centralized infrastructure storage

RAIDZ2 provides tolerance for up to two disk failures within each RAIDZ2 vdev.

The additional hot spares provide replacement capacity for eligible failed disks.

### Backup Pool — hdd-backup

The backup pool consists of a four-disk RAIDZ1 configuration.

It provides dedicated storage for infrastructure backup repositories, including Veeam Backup & Replication.

Separating backup storage from the primary VM storage pool provides organizational and operational isolation between production data and backup repositories.

The backup pool remains within the same physical TrueNAS environment and therefore does not constitute an independent offsite backup.

### SSD Pool — ssd-scratch

The SSD pool consists of a single SSD and provides storage for virtual machines requiring faster disk access.

This pool is exposed to Proxmox through an iSCSI-backed virtual disk resource.

Because the pool consists of a single disk, it does not provide disk-level redundancy.

The SSD tier is used with an understanding of its capacity and availability limitations.

## Storage Protocols

TrueNAS provides three primary storage protocols.

### iSCSI — Block Storage

iSCSI provides block-level storage to the Proxmox hypervisor.

Two storage tiers are configured:

| Proxmox Storage | TrueNAS Pool | Zvol | Purpose |
|---|---|---|---|
| vm-hdd | hdd-main | iscsi/proxmox-vmstore | HDD-backed VM disks |
| vm-ssd | ssd-scratch | proxmox-ssd | SSD-backed VM disks |

The HDD-backed storage provides capacity-oriented VM storage, while the SSD-backed storage supports workloads that benefit from lower storage latency.

### NFS — File-Based Storage

NFS provides centralized installation media storage for Proxmox.

The existing TrueNAS dataset `hdd-main/vms` serves as the NFS export used by the Proxmox `ISO` storage resource.

Historically, this dataset supported both virtual machine disks and installation media.

Following storage reorganization, virtual machine disks were moved to dedicated iSCSI-backed storage resources, leaving NFS primarily responsible for installation media.

### SMB — File Sharing and Backup Repositories

SMB provides network-accessible storage for infrastructure and personal data.

Configured shares include:

| Share Purpose | Description |
|---|---|
| Infrastructure Backups | Veeam backup repository |
| Archived Desktop Data | Preserved copy of a retired desktop's original disk contents |
| Personal File Storage | Network-accessible family file storage |

These shares provide separation between infrastructure recovery data, archival information, and personal storage.

## Storage Architecture Evolution

The storage environment has undergone several architectural improvements.

### Compute and Storage Separation

The original multi-node Proxmox cluster was consolidated into a standalone virtualization host.

The second server was repurposed for TrueNAS, creating a dedicated centralized storage platform.

### Storage Tier Reorganization

Proxmox storage resources were reorganized to make workload placement and underlying storage technology easier to identify.

The resulting naming convention distinguishes:

- `vm-hdd` — HDD-backed virtual machine storage
- `vm-ssd` — SSD-backed virtual machine storage
- `ISO` — Centralized installation media

This reduces ambiguity during virtual machine provisioning, troubleshooting, and resource management.

### Storage Protocol Separation

The original NFS storage arrangement supported multiple workload types.

Virtual machine disks were subsequently separated into dedicated iSCSI-backed storage resources, while NFS was retained for installation media.

This provides clearer workload organization and storage administration.

### Hardware Repurposing

An earlier mirrored SSD storage pool was retired, and its drives were repurposed for another infrastructure project.

This reduced unnecessary hardware consumption while allowing the remaining storage environment to continue supporting its intended workloads.

## Storage Maintenance and Health

TrueNAS provides centralized visibility into ZFS pool health, capacity utilization, and disk status.

Storage maintenance includes:

- Scheduled ZFS scrubs
- Pool health monitoring
- Disk error monitoring
- Capacity utilization reviews
- Storage resource administration
- Backup integration with Veeam

The primary HDD pool uses RAIDZ2 redundancy and configured hot spares to improve resilience against disk failures.

RAID redundancy protects against certain hardware failures but does not replace independent backups.

## Backup and Recovery Integration

Veeam Backup & Replication integrates with Proxmox VE to provide virtual machine-level backup and recovery.

Backup data is stored on the dedicated `hdd-backup` pool through an SMB repository.

This separates backup data from the primary VM storage pool and provides a centralized location for infrastructure recovery.

The Veeam architecture, backup job configuration, retention policies, and recovery procedures are documented separately under Backup Infrastructure.

## Capacity Planning and Future Improvements

Planned storage improvements include:

- Expanding SSD-backed storage capacity
- Evaluating additional redundant SSD storage
- Improving storage performance visibility
- Continuing capacity and utilization monitoring
- Evaluating additional backup redundancy and offsite recovery options
- Optimizing storage placement based on workload requirements

SSD expansion is a priority for improving performance in resource-intensive virtual machines and network simulation environments.

## Future Technical Documentation

Detailed storage implementation and configuration will be maintained in a dedicated technical repository.

Planned topics include:

- TrueNAS installation and configuration
- ZFS pool architecture and redundancy
- iSCSI targets, extents, and Proxmox integration
- NFS exports and storage permissions
- SMB shares and access controls
- Storage performance and troubleshooting
- Backup repository configuration
- Storage migration and capacity planning
