# Storage

## Overview

This document describes the storage architecture of the HuntWired homelab.

## Storage Hardware

### TrueNAS Scale

- Dell R620
- TrueNAS Scale
- Dedicated storage server

### Expansion Shelves

- Dell SC200 Expansion Shelves

## Storage Pools

### hdd-main

Primary storage pool used for virtual machine storage.

### hdd-backup

Dedicated backup storage pool.

### ssd-mirror

High-speed mirrored SSD storage.

### ssd-scratch

Scratch and temporary storage pool.

## Storage Services

### iSCSI

- Presented to Proxmox
- Virtual machine storage
- Shared storage architecture

### NFS

- ISO repository
- Shared file storage

### SMB

- Veeam backup repository
- Network file shares

## Storage Integration

### Proxmox

- iSCSI connected storage
- VM disk storage
- Centralized storage management

### Veeam

- SMB backup repository
- Automated backup storage

## Future Improvements

- Additional monitoring
- Capacity planning
- Storage performance reporting
