# Remote Access & Secure Administration

## Overview

The HuntWired homelab uses multiple remote-access technologies to provide private network connectivity, remote infrastructure administration, and externally accessible applications.

The architecture combines **Tailscale, Cloudflare Tunnel, AnyDesk, and Cisco network ACLs** to support different access requirements while maintaining internal network segmentation.

The primary design objectives are:

- Enable remote administration of homelab infrastructure
- Minimize unnecessary inbound firewall exposure
- Restrict management access to authorized devices
- Provide encrypted connectivity to internal networks
- Publish selected web applications
- Support remote administration from mobile devices
- Separate public-facing services from infrastructure management

Public hostnames, internal addressing, subnet ranges, and detailed access-control rules are intentionally excluded from this documentation.

## Remote Access Architecture

| Technology | Primary Function | Access Method |
|---|---|---|
| Tailscale | Private network connectivity | Encrypted VPN and subnet routing |
| Cloudflare Tunnel | External application publishing | HTTPS through Cloudflare |
| AnyDesk | Remote desktop administration | Remote control of management workstation |
| Cisco Catalyst 3850 | Internal network segmentation | Layer 3 ACL-based access restrictions |

Each technology serves a distinct purpose.

Tailscale provides private network connectivity, Cloudflare Tunnel publishes selected applications, and AnyDesk provides remote control of the primary management workstation.

The Cisco core switch enforces internal network access restrictions.

## Tailscale

Tailscale provides encrypted private connectivity between authorized devices and selected internal homelab networks.

### Enrolled Devices

Three devices are enrolled in the Tailscale network:

- Personal management laptop
- Mobile phone
- ZimaBoard 832

### ZimaBoard Subnet Router

The ZimaBoard functions as a Tailscale subnet router, advertising five internal homelab networks to authorized remote clients.

The approved routes cover infrastructure such as:

- Virtualization systems
- Storage infrastructure
- Internal servers
- Administrative resources
- Monitoring services

This design allows remote clients to reach selected internal networks without installing Tailscale on every server.

Route approval alone does not grant unrestricted access. Connectivity remains subject to Tailscale policies, client routing, and internal network ACLs.

### Design Benefits

- Encrypted connectivity between enrolled devices
- Remote access without exposing individual management ports
- Centralized subnet routing through the ZimaBoard
- Reduced need for VPN software on individual infrastructure systems
- Access to multiple internal networks through a single routing device

## Cloudflare Tunnel

Cloudflare Tunnel provides externally accessible hostnames for selected homelab web applications.

The tunnel establishes outbound connectivity to Cloudflare, allowing published applications to be reached without traditional inbound firewall port forwarding.

### Published Applications

Eight application routes are configured.

| Application | Purpose |
|---|---|
| Proxmox VE | Virtualization management |
| Jellyfin | Remote media streaming |
| TrueNAS SCALE | Storage administration |
| Dashy | Experimental homelab landing page |
| GNS3 | Network simulation and lab management |
| Uptime Kuma | Service availability reporting |
| Apache Guacamole | Browser-based VNC console access for GNS3 |
| Grafana | Infrastructure monitoring dashboards |

### Application Use Cases

**Infrastructure Management**

Proxmox and TrueNAS provide browser-based interfaces for virtualization and storage administration.

**Network Lab Administration**

GNS3 provides a remotely accessible network simulation environment.

Apache Guacamole provides browser-based VNC console access for devices within the GNS3 lab.

**Monitoring**

Grafana provides access to infrastructure performance dashboards.

Uptime Kuma provides a status page that family and friends can use to check service availability.

**Media Services**

Jellyfin provides remote media streaming for authorized users.

**Homelab Landing Page**

Dashy was deployed as an experimental centralized landing page. Development is currently paused, and the dashboard is not considered complete.

### Security Architecture

Cloudflare Tunnel reduces the need to expose internal services through inbound firewall port forwarding.

However, tunnel publishing and application authentication are separate security functions.

Cloudflare Access application policies have not yet been implemented.

Published services currently depend on their respective application-level authentication and security configurations.

For example, Grafana uses its native login interface.

### Security Considerations

Future security improvements may include:

- Evaluating Cloudflare Access for administrative applications
- Implementing additional authentication controls
- Reviewing application-level permissions
- Restricting access to sensitive management interfaces
- Periodically auditing published services
- Reviewing the necessity of each externally accessible application

## AnyDesk

AnyDesk provides remote desktop access to the primary management workstation.

### Deployment

AnyDesk is installed on:

- Personal management laptop
- Mobile phone

### Management Workstation Architecture

The management laptop serves as the primary administrative workstation for the homelab.

Restrictive Cisco ACLs limit management connectivity between internal network segments.

The management laptop is permitted to access infrastructure resources that are not broadly accessible from other devices.

When away from home, AnyDesk allows remote control of the management laptop from the phone.

This provides an administrative access path through an already authorized workstation rather than requiring unrestricted direct management access from mobile devices.

### Administrative Access Workflow

1. Establish an AnyDesk session from the remote device.
2. Connect to the management laptop.
3. Use the laptop's existing authorized network connectivity.
4. Access permitted infrastructure management interfaces.
5. Perform administrative tasks through the established workstation.

The management laptop effectively functions as a remote administration jump workstation.

### Security Considerations

Because the management laptop has elevated network access, protecting the workstation and its remote-access configuration is especially important.

Recommended controls include:

- Strong authentication
- Multifactor authentication where supported
- Restricting unattended remote access
- Keeping the workstation and remote-access software updated
- Limiting administrative privileges
- Reviewing remote-access permissions
- Maintaining restrictive network ACLs

A future dedicated travel laptop may provide a more convenient remote administration endpoint.

## Network Segmentation and Access Control

Internal VLAN routing and access control are handled by the Cisco Catalyst 3850 core switch.

The FortiGate provides perimeter security, while the Catalyst switch enforces internal Layer 3 access restrictions.

### Administrative Access Principles

- Restrict management interfaces to authorized sources
- Maintain separation between user, server, and infrastructure networks
- Use a designated management workstation
- Limit communication between internal network segments
- Avoid unnecessarily exposing management services
- Preserve network segmentation when implementing remote access

### Defense-in-Depth Considerations

The remote-access architecture uses several complementary mechanisms:

**Private Connectivity**

Tailscale provides encrypted connectivity to enrolled devices and approved internal networks.

**Application Publishing**

Cloudflare Tunnel provides externally accessible application routes without traditional inbound port forwarding.

**Administrative Workstation**

AnyDesk enables remote control of the management laptop, which already has authorized management connectivity.

**Internal Access Restrictions**

Cisco ACLs enforce traffic restrictions between internal network segments.

These mechanisms provide different access paths and should not be treated as interchangeable security controls.

## Operational Use Cases

### Remote Infrastructure Administration

Use AnyDesk to access the management workstation and administer infrastructure through existing ACL-permitted connections.

### Private Network Access

Use Tailscale to establish encrypted connectivity to enrolled devices and approved internal networks.

### Remote GNS3 Access

Access the GNS3 web interface through Cloudflare Tunnel and use Apache Guacamole for browser-based VNC console access.

### Remote Monitoring

Access Grafana dashboards remotely to review infrastructure performance and resource utilization.

### Service Availability Reporting

Provide Jellyfin users with a published Uptime Kuma status page to independently verify service availability.

### Remote Media Streaming

Provide authorized family members and friends with remote access to the Jellyfin media server.

## Future Improvements

- Evaluate Cloudflare Access policies for sensitive management applications
- Review authentication and authorization across published services
- Evaluate additional Tailscale access restrictions
- Review AnyDesk security and management workstation protections
- Acquire a dedicated travel laptop for remote administration
- Revisit the centralized Dashy landing-page project when higher-priority work is complete
- Document additional access-control validation and security testing procedures

## Technical Skills Demonstrated

**Remote Connectivity:** Tailscale, VPN Subnet Routing, AnyDesk, Cloudflare Tunnel

**Network Security:** Cisco ACLs, VLAN Segmentation, Access Control, Administrative Network Isolation

**Infrastructure Administration:** Proxmox VE, TrueNAS SCALE, GNS3, Apache Guacamole

**Operational Security:** Remote Management, Application Authentication, Reduced Firewall Exposure, Management Workstation Design

**Architecture:** Multi-Method Remote Access, Secure Administration Workflows, Service Publishing, Network Segmentation
