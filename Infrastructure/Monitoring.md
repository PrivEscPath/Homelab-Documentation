# Infrastructure Monitoring & Observability

## Overview

The HuntWired homelab uses a centralized monitoring and observability stack to track infrastructure health, resource utilization, network activity, and service availability.

The monitoring environment is hosted primarily on a dedicated ZimaBoard running Debian 13 and Docker.

The implementation provides:

- Centralized infrastructure metrics collection
- Real-time Grafana dashboards
- Historical CPU, memory, disk, and network performance metrics
- Proxmox hypervisor and virtual machine monitoring
- TrueNAS storage and ZFS performance visibility
- Service availability monitoring
- A self-service status page for Jellyfin users
- Remote access to monitoring dashboards

## Monitoring Architecture

| Component | Technology | Function |
|---|---|---|
| Monitoring Host | ZimaBoard 832 | Dedicated monitoring infrastructure |
| Operating System | Debian 13 | Linux container host |
| Container Platform | Docker | Monitoring service deployment |
| Metrics Collection | Prometheus | Collection and storage of time-series metrics |
| Visualization | Grafana OSS | Infrastructure dashboards |
| Host Metrics | Node Exporter | Linux system performance |
| Virtualization Metrics | Proxmox Exporter | Hypervisor and VM monitoring |
| Metrics Integration | Graphite Exporter | Graphite-compatible metric ingestion |
| Availability Monitoring | Uptime Kuma | Service health and status reporting |
| Log Analysis | Splunk Enterprise | Deployed; log ingestion pending |

### Metrics Collection Architecture

The monitoring architecture separates metric collection, storage, and visualization.

**Monitored Infrastructure → Exporters → Prometheus → Grafana**

Prometheus collects metrics from configured exporters, while Grafana queries the Prometheus data source to display infrastructure performance.

This architecture allows multiple systems to be monitored from a centralized interface without requiring administrators to log into individual servers for routine performance checks.

## ZimaBoard Monitoring Host

The ZimaBoard 832 serves as the primary monitoring platform.

**Configuration:**

- Operating System: Debian 13
- Container Runtime: Docker
- Dedicated monitoring services
- Centralized metrics collection and visualization
- Independent physical monitoring host

### Docker Services

The following containers are deployed and operational:

| Container | Purpose |
|---|---|
| grafana | Dashboards and visualization |
| prometheus | Time-series metrics collection |
| node-exporter | Linux host monitoring |
| proxmox-exporter | Proxmox infrastructure metrics |
| graphite-exporter | Graphite metric integration |
| uptime-kuma | Service availability monitoring |
| ansible | Infrastructure automation |

Ansible is hosted on the same platform but serves a separate infrastructure automation function.

## Grafana

Grafana OSS provides centralized visualization of infrastructure performance metrics.

Four operational dashboards are configured within the HomeLab Monitoring folder.

### 1. Proxmox Dashboard

Provides visibility into virtualization infrastructure, including:

- Hypervisor CPU and memory utilization
- Individual virtual machine resource consumption
- Host and VM uptime
- Storage utilization
- Infrastructure performance trends

This dashboard supports capacity planning and troubleshooting of resource-intensive workloads.

### 2. Jellyfin Dashboard

Provides performance visibility into the physical Jellyfin media server.

Monitored metrics include:

- CPU specifications and utilization
- Installed and utilized memory
- System uptime
- Disk utilization
- Network transmission and reception
- Individual network interface activity

These metrics help identify infrastructure resource constraints affecting media services.

### 3. TrueNAS Dashboard

Provides storage infrastructure observability, including:

- CPU utilization
- Memory availability
- System uptime
- SMART disk temperature metrics
- ZFS Adaptive Replacement Cache (ARC) behavior
- Disk read/write activity
- Storage I/O trends

Monitoring ZFS ARC and disk I/O is particularly useful for understanding storage performance and identifying resource contention during backup and virtualization workloads.

### 4. Node Exporter Full

A reusable Linux system monitoring dashboard using Prometheus Node Exporter metrics.

The dashboard supports selecting monitored hosts and viewing:

- CPU utilization and load
- Memory consumption
- Swap usage
- Disk capacity and filesystem utilization
- Network throughput
- System uptime
- Detailed operating system performance metrics

Verified monitored instances include the ZimaBoard and Jellyfin server.

## Prometheus

Prometheus serves as the central metrics collection and time-series storage platform.

### Configured Exporters

**Node Exporter**

Collects Linux operating system performance metrics from monitored hosts, including ZimaBoard and Jellyfin.

**Proxmox Exporter**

Provides virtualization-related metrics for Proxmox hosts and virtual machines.

**Graphite Exporter**

Provides an ingestion endpoint for Graphite-compatible metrics. The container is operational; individual upstream metric producers have not been independently verified.

### Monitoring Benefits

Centralized metric collection supports:

- Historical performance analysis
- Infrastructure troubleshooting
- Resource utilization trends
- Virtualization capacity planning
- Storage performance analysis
- Cross-system dashboard visualization

## Uptime Kuma

Uptime Kuma provides lightweight service availability monitoring independently of Grafana's detailed performance dashboards.

### Monitored Services

| Service | Monitoring Purpose |
|---|---|
| Jellyfin | Media server availability |
| Proxmox Cloud Node | Virtualization host availability |

Both monitored services were operational during the latest verification.

### Self-Service Status Page

A service status page is available for family and friends who use the Jellyfin media server.

The page provides:

- Overall service operational status
- Individual service availability
- Recent uptime history
- A simple interface for identifying service outages

The status page allows users to check whether Jellyfin or its supporting infrastructure is experiencing an outage before contacting the administrator.

This reduces unnecessary support requests and provides service consumers with direct visibility into availability.

## Remote Monitoring Access

Grafana is accessible remotely through a published hostname.

The current implementation uses Grafana's native login authentication to restrict dashboard access.

Remote availability allows infrastructure performance and health to be reviewed without requiring direct access to the local monitoring workstation.

### Security Considerations

Potential future improvements include:

- Adding Cloudflare Access as an additional authentication layer
- Reviewing externally exposed application permissions
- Periodically reviewing Grafana user accounts
- Applying routine security updates to monitoring containers

## Splunk Enterprise — Planned Integration

A dedicated Ubuntu Server virtual machine has been deployed with Splunk Enterprise.

The platform is installed, but log ingestion and source integrations have not yet been configured.

### Planned Work

- Configure log collection from infrastructure devices
- Ingest Linux and Windows event logs
- Explore firewall and network security log ingestion
- Build searchable event datasets
- Develop security monitoring dashboards
- Configure event-based alerting

This project is scheduled for further development after completion of the GitHub portfolio documentation.

## Operational Use Cases

### Virtualization Capacity Monitoring

Grafana provides visibility into Proxmox host and VM resource consumption, helping identify CPU, memory, and storage constraints.

### Storage Performance Analysis

TrueNAS monitoring provides visibility into ZFS ARC behavior, disk activity, and drive temperatures.

These metrics can be used when investigating performance bottlenecks affecting virtual machines and backup operations.

### Media Server Monitoring

Jellyfin resource monitoring provides visibility into server health and network activity, while Uptime Kuma independently reports service availability.

### Remote Infrastructure Visibility

Published Grafana access enables performance monitoring while away from the homelab.

### User-Facing Service Status

The Uptime Kuma status page provides family and friends with a simple way to verify Jellyfin availability without needing access to administrative monitoring tools.

## Future Improvements

- Complete Splunk Enterprise log ingestion and dashboard configuration
- Expand infrastructure monitoring coverage where needed
- Implement additional performance and availability alerts
- Evaluate centralized log correlation and security event monitoring
- Add further authentication protection to remote Grafana access
- Develop additional dashboards for security and network infrastructure
- Document monitoring configuration and deployment automation

## Technical Skills Demonstrated

**Monitoring & Observability:** Grafana, Prometheus, Node Exporter, Proxmox Exporter, Graphite Exporter, Uptime Kuma

**Infrastructure:** Debian Linux, Docker, Proxmox VE, TrueNAS SCALE, ZFS, Jellyfin

**Operational Practices:** Infrastructure Monitoring, Metrics Collection, Time-Series Analysis, Capacity Planning, Service Availability, Performance Troubleshooting, Remote Monitoring, Status Page Management

**Future Security Integration:** Splunk Enterprise, Centralized Log Collection, Security Event Analysis
