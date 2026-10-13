# Backup and Recovery Infrastructure

## Overview

My homelab uses **Veeam Backup & Replication 13 Community Edition** to protect critical infrastructure workloads hosted on Proxmox VE.

Backup data is stored on a centralized TrueNAS SCALE SMB repository using HDD-backed storage.

The backup strategy focuses on:

- Automated daily backups
- Application-aware protection for Active Directory
- Retention and synthetic full backups
- Resource-conscious scheduling
- Tested virtual machine and file-level recovery
- Email monitoring and failure notifications

## Infrastructure Architecture

| Component | Technology |
|---|---|
| Backup Management | Veeam Backup & Replication 13 |
| Backup Server OS | Windows Server 2022 |
| Hypervisor | Proxmox VE |
| Backup Transport | Veeam Proxmox Worker / HotAdd |
| Backup Repository | TrueNAS SCALE SMB |
| Repository Storage | HDD-backed |
| Network Segmentation | Cisco Catalyst 3850 ACLs |

Veeam manages backup operations through its Proxmox integration, using a dedicated worker to process virtual machine disk data.

## Backup Jobs

| Job | Schedule | Status |
|---|---|---|
| DC2 Daily | 8:00 AM | Successful |
| Veeam Server Backup | 9:00 AM | Successful |
| GNS3 Daily | 10:00 AM | Previously successful |
| DC1 & DC2 Entire Computer | Disabled | Legacy job |

### Scheduling Strategy

Backup jobs are staggered by one hour to reduce concurrent storage operations against the HDD-backed TrueNAS repository.

This approach helps manage disk I/O contention while balancing backup completion times and available infrastructure resources.

Job durations are monitored because staggered start times do not guarantee that backup operations will never overlap.

## Backup Retention

The daily backup strategy uses:

- **Retention:** 14 days
- **Backup method:** Daily incremental backups
- **Synthetic full:** Every Saturday
- **Repository:** TrueNAS SCALE SMB

A separate golden backup of the GNS3 server was also created before additional configuration changes, providing a known-good recovery point.

## Application-Aware Processing

The DC2 backup job uses Veeam application-aware guest processing to support consistent backups of the Active Directory domain controller.

The implementation combines Windows guest processing with Proxmox HotAdd transport for virtual disk backup operations.

## Recovery Testing

Backup functionality was validated using a Windows 11 test workstation.

### Full Virtual Machine Recovery

A complete virtual machine restore was performed successfully. The recovered Windows 11 VM was booted to verify that the restored system was functional.

### File-Level Recovery

An individual test file was recovered successfully from backup, validating file-level restoration capabilities.

These tests demonstrate both full workload recovery and granular data recovery.

## Monitoring and Notifications

Veeam global email notifications are configured and operational.

Validation includes:

- Successful SMTP test message
- Confirmed delivery of DC2 backup failure notifications
- Backup job status monitoring through the Veeam console

Email delivery and failure notifications are verified. Successful-job notifications for every new job have not been individually confirmed.

## Troubleshooting: DC2 Backup Failure

### Problem

A newly configured Proxmox backup job failed during Windows guest processing.

Veeam reported an inability to connect to the guest operating system and install guest processing components.

### Investigation

Connectivity and authentication troubleshooting included:

1. Testing SMB connectivity on TCP 445.
2. Testing RPC Endpoint Mapper connectivity on TCP 135.
3. Verifying authenticated administrative share access.
4. Reviewing Cisco Catalyst 3850 inter-VLAN ACL restrictions.
5. Adding a host-specific ACL exception for the Proxmox backup worker.
6. Updating guest-processing credentials to explicitly identify the Active Directory domain.

### Resolution

Following the network ACL and credential configuration changes, the DC2 backup completed successfully with application-aware processing enabled.

Because both changes preceded the successful backup, the precise individual cause was not conclusively isolated.

### Lessons Learned

Successful credential verification does not guarantee that every guest-processing operation will succeed during a backup.

Backup troubleshooting requires examining the complete communication path between the management server, backup worker, hypervisor, and protected workload.

The worker's broad host-specific ACL exception should be reviewed and narrowed to required traffic where practical.

## Future Improvements

- Continue monitoring backup duration and HDD repository performance.
- Periodically repeat VM and file-level recovery tests.
- Validate application-aware Active Directory recovery procedures.
- Review notification behavior after modifying backup jobs.
- Evaluate secondary or offsite backup storage.
- Verify ongoing preservation of the GNS3 golden backup.

## Technical Skills Demonstrated

Veeam Backup & Replication · Proxmox VE · Windows Server · Active Directory · TrueNAS SCALE · SMB Storage · Application-Aware Backups · HotAdd Transport · Backup Retention · Virtual Machine Recovery · File-Level Recovery · Cisco ACL Troubleshooting · Backup Monitoring
