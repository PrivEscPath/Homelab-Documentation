# Backups

## Overview

This document describes the backup and recovery strategy used within the HuntWired homelab.

## Backup Platform

### Veeam Backup & Replication

- Veeam Community Edition
- Windows Server 2022
- Virtual Machine Hosted on Proxmox

## Backup Repository

### TrueNAS Scale

- SMB Share Repository
- Dedicated Backup Storage
- Centralized Backup Management

## Protected Systems

### Domain Controllers

#### DC1

- Entire Computer Backup
- Physical Domain Controller

#### DC2

- Entire Computer Backup
- Virtual Domain Controller

## Backup Schedule

### Daily Backups

- Incremental Backups
- Automated Scheduling

### Weekly Backups

- Synthetic Full Backup
- Long-Term Recovery Points

## Notifications

### Email Alerts

- Backup Success Notifications
- Backup Failure Notifications
- Job Status Reporting

## Recovery Objectives

### System Recovery

- Bare Metal Recovery
- File Level Recovery
- Application Consistent Backups

## Backup Architecture

Veeam → SMB Repository on TrueNAS → Dedicated Backup Storage

## Future Improvements

- Additional protected systems
- Recovery testing documentation
- Off-site backup strategy
