# Infrastructure Overview

This section documents the core home lab infrastructure build.

## Hardware Overview
- Virtualization hosts
- Media Servers
- Active Directory
- Backups
- Network equipment
- Storage systems
- Monitoring

### Virtualization
  - Dell R620
  - Proxmox 9.1.7
  - Node configuration
  - Storage integration
  - VLAN Trunking

###Hosted Services
  - DC2
  - Veeam
  - CloudflareLXC
  - Test Workstation
  - Ethical Hacking Lab ( Domain Controller, 2x Workstations, Linux Workstation)

## Media Server
- Dell R720XD
- Jellyfin
- Debian 13

## Active Directory
### Domain Ino
  - ad.huntwired.com
  - DNS Integrated Active Directory
  
### Domain Controllers 
  - DC1 Windows Server 2022
  - HP DL360 G7
### DC2 Windows Server 2022
  - VM in Proxmox

### Organizational Unit Structure
  - Domain Admins
  - Users
  - Workstations
  - Servers

### Group Policy
  - ICMP Enabled for Mgmt
  - RDP Access Policies
  - Domain Security Policies

### Services
  - Active Directory Domain Services (AD DS)
  - DNS
  - Group Policy
  - Authentication Services

## Backups
-  Veeam
-  VM in Proxmox
-  Backups Stored on share from TrueNAS
  
## Networking
### Cisco Catalyst 3850
  - VLAN segmentation strategy
  - LACP port channels
  - Layer 3 Routing
  - Inter-VLAN routing
  - ACL-based routing
  - Mgmt VLAN isolation
    
### Fortigate Firewall
  - Static Routes
  - Policy based routing

## Storage
- Dell R620 running TrueNAS Scale
- 2x Dell SC2000 Shelves 
- iSCSI presentation to Proxmox
- NFS ISO Repository
- Veeam Backup Repo
- SSD on iSCSI

### Storage Pools
  - hdd-main
  - hdd-backup
  - ssd-scratch

# Monitoring
###Grafana
  - Custom Proxmox dashboards
  - Custom Jellfin dashboards
  - Custom TrueNAS dashboards

### Prometheus
  - Node Exporter
  - Proxmox Exporter
  - Truenas Exporter

### Uptime Kuma
  - Service Monitoring
  - Status Page
  - Infrastructure Alerting
