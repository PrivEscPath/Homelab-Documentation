# Monitoring

## Overview

This document describes the monitoring platform used within the HuntWired homelab.

## Monitoring Platform

### ZimaBoard

- Dedicated monitoring host
- Debian 13
- Docker services

## Grafana

### Purpose

Provides centralized dashboards and infrastructure visibility.

### Dashboards

- Proxmox
- Jellyfin
- ZimaBoard
- Infrastructure Monitoring

### Metrics

- CPU Utilization
- Memory Utilization
- Storage Usage
- Network Throughput
- System Uptime

## Prometheus

### Purpose

Collects and stores infrastructure metrics.

### Exporters

#### Node Exporter

- ZimaBoard
- Jellyfin
- Linux Hosts

#### Proxmox Exporter

- Proxmox Host Metrics
- Virtual Machine Metrics

## Uptime Kuma

### Purpose

Service availability monitoring.

### Monitored Systems

- FortiGate Firewall
- Cisco Catalyst 3850
- Proxmox
- TrueNAS
- Jellyfin
- DC1
- DC2
- Veeam

## Alerting

- Service Availability Monitoring
- Status Page Reporting

## Future Improvements

- Additional dashboards
- Enhanced alerting
- Log aggregation
