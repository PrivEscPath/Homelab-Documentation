# Media Server Infrastructure — Jellyfin

## Overview

The HuntWired homelab includes a dedicated, self-hosted Jellyfin media server providing movies, television, collections, and live television to authorized family members and friends.

The platform runs directly on a Dell PowerEdge R720xd with Debian 13, using locally attached hardware RAID storage.

The environment is designed around:

- Dedicated bare-metal Linux hosting
- Redundant local media storage
- Direct Play media streaming
- Secure remote service publishing
- Network segmentation and access controls
- Individual user accounts and parental restrictions
- Centralized performance and availability monitoring

## Hardware Infrastructure

| Component | Configuration |
|---|---|
| Server | Dell PowerEdge R720xd |
| Deployment | Bare metal |
| Operating System | Debian 13 |
| Media Application | Jellyfin |
| Installation Method | Native Linux installation |
| Storage Controller | Broadcom / LSI MegaRAID SAS 2208 |
| Physical Storage | 12 × 2 TB HDDs |
| Active Array Drives | 11 |
| Hot Spare | 1 × 2 TB HDD |
| Media Filesystem | ext4 |
| Usable Media Volume | Approximately 16.4 TiB |
| Dedicated GPU | None |

The server operates independently of the Proxmox virtualization environment, separating media workloads from virtualized infrastructure services.

## Storage Architecture

The R720xd uses a hardware RAID controller to present its locally attached media storage as a single logical volume.

### Disk Configuration

- Twelve physical 2 TB hard drives
- Eleven drives participating in the active array
- One dedicated hot spare
- Approximately 16.4 TiB of usable storage
- ext4 filesystem
- Operating system installed separately from the media volume

The available capacity is consistent with a RAID 6 configuration, although the exact RAID level has not yet been verified through the controller management interface.

### Storage Design Considerations

The storage architecture prioritizes:

- Drive fault tolerance
- Hot spare availability
- Large local media capacity
- Independence from network-attached storage
- Separation of operating system and media storage
- Simplified filesystem administration

Using dedicated local storage avoids introducing network storage dependencies into normal media playback.

RAID redundancy improves availability during supported drive failures but does not replace independent backups.

## Jellyfin Deployment

Jellyfin is installed natively on Debian 13 rather than deployed in Docker or a virtual machine.

The dedicated bare-metal environment provides direct access to local storage and eliminates the need for an additional virtualization or container layer.

### Media Organization

The Jellyfin environment includes four primary library sections.

| Library | Purpose |
|---|---|
| Movies | On-demand movie library |
| TV Shows | Episodic television content |
| Live TV | Streaming television channels |
| Collections | Organized media groupings |

Jellyfin provides metadata management, artwork, library browsing, and playback interfaces for supported client devices.

## Playback Architecture

The server is intentionally configured around **Direct Play**, without server-side transcoding.

### Design Decisions

- No dedicated GPU
- No hardware transcoding
- Server-side transcoding is not used
- Client devices are expected to support the original media formats
- Playback compatibility is handled primarily at the client level

### Operational Benefits

Direct Play avoids CPU-intensive video conversion during playback.

This reduces server processing requirements and eliminates the need for dedicated transcoding hardware.

The tradeoff is reduced compatibility with clients that cannot decode the original media formats.

This is an intentional design decision based on the devices supported by the environment.

## Network Architecture

The Jellyfin server resides on a dedicated internal VLAN.

Internal routing and access restrictions are managed by a Cisco Catalyst 3850 core switch.

### Network Design

- Dedicated media-server network segment
- Layer 3 routing through the Cisco core switch
- ACL-based restrictions between internal VLANs
- Separation from infrastructure management networks
- External service publishing through Cloudflare Tunnel

The design separates media streaming from sensitive administrative and infrastructure workloads.

### Remote Access

Jellyfin is published through Cloudflare Tunnel.

This allows authorized users to access the service remotely without requiring a traditional inbound firewall port-forwarding rule.

User authentication and media permissions are managed through Jellyfin's native account system.

Cloudflare Access application policies are not currently implemented.

Public hostnames, private management addresses, and detailed access-control configurations are intentionally excluded from this documentation.

## User Management and Parental Controls

Jellyfin provides individual accounts for family members and friends.

### Account Administration

Administrative responsibilities include:

- Creating and maintaining user accounts
- Managing library permissions
- Configuring user-specific access
- Maintaining children's profiles
- Applying content-rating restrictions

### Parental Restrictions

Children's accounts are configured with maximum permitted content ratings based on parental preferences.

These restrictions allow age-appropriate content access without requiring separate physical media servers.

Account permissions and parental restrictions are managed centrally through the Jellyfin administrative interface.

## Live Television

Live TV functionality uses Pluto TV as a streaming source.

The integration provides access to live television content through Jellyfin.

Electronic Program Guide (EPG) configuration is still in progress.

Future work includes improving channel guide data and program information.

## Monitoring and Observability

The Jellyfin server is integrated into the homelab's centralized monitoring infrastructure.

### Prometheus and Grafana

Node Exporter collects Linux host performance metrics for Prometheus.

Grafana provides dashboards displaying:

- CPU utilization
- Memory consumption
- System uptime
- Disk utilization
- Network transmission and reception
- Historical resource consumption

These metrics support capacity planning, performance troubleshooting, and identifying potential resource constraints.

### Uptime Kuma

Uptime Kuma independently monitors Jellyfin service availability.

A published status page allows family members and friends to check whether the service is operational before contacting the administrator.

This provides self-service outage visibility and reduces unnecessary support requests.

## Operational Troubleshooting

Ongoing administration includes investigating playback issues, monitoring infrastructure performance, and maintaining reliable service availability.

Examples of troubleshooting activities include:

- Investigating audio synchronization issues
- Evaluating client-specific playback behavior
- Reviewing media format compatibility
- Monitoring network utilization during playback
- Identifying storage and resource bottlenecks
- Maintaining media metadata and library organization

Centralized monitoring provides historical performance data to support troubleshooting.

## Future Improvements

- Verify the hardware RAID level and controller health reporting
- Complete Pluto TV electronic program guide configuration
- Review long-term storage capacity and expansion options
- Improve monitoring and alerting where appropriate
- Review user permissions and parental restrictions as requirements evolve
- Continue evaluating playback reliability and client compatibility

## Technical Skills Demonstrated

**Systems Administration:** Debian Linux, Bare-Metal Server Deployment, Linux Service Management

**Storage Infrastructure:** Hardware RAID, Hot Spare Configuration, ext4, Local Storage Administration

**Networking:** VLAN Segmentation, Cisco ACLs, Cloudflare Tunnel, Remote Service Publishing

**Monitoring:** Prometheus, Grafana, Node Exporter, Uptime Kuma

**Application Administration:** Jellyfin, User Management, Access Restrictions, Parental Controls

**Operations:** Performance Troubleshooting, Capacity Planning, Service Availability Monitoring, Client Compatibility
