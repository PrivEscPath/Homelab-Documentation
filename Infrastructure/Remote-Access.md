# Remote Access & Secure Administration

## Overview

The HuntWired homelab uses multiple remote-access technologies to provide secure administration, private network connectivity, and externally accessible applications.

The architecture combines Tailscale, Cloudflare Tunnel, and AnyDesk to support different access requirements while maintaining restrictive internal network access controls.

The primary design objectives are:

- Enable remote infrastructure administration
- Minimize unnecessary inbound firewall exposure
- Restrict management access through network ACLs
- Provide encrypted connectivity to internal networks
- Publish selected web applications
- Support administration from mobile devices
- Separate public-facing services from administrative access

## Remote Access Architecture

| Technology | Primary Function | Access Method |
|---|---|---|
| Tailscale | Private network connectivity | Encrypted mesh VPN and subnet routing |
| Cloudflare Tunnel | External application publishing | HTTPS through Cloudflare |
| AnyDesk | Remote desktop administration | Remote control of management workstation |
| Cisco Catalyst 3850 ACLs | Internal network segmentation | Layer 3 access restrictions |

These technologies serve complementary purposes rather than providing identical access methods.

## Tailscale

Tailscale provides encrypted private connectivity between authorized devices and selected homelab networks.

### Enrolled Devices

Three devices are enrolled in the Tailscale network:

- Personal management laptop
- Mobile phone
- ZimaBoard 832

### ZimaBoard Subnet Router

The ZimaBoard functions as a Tailscale subnet router, advertising internal homelab networks to authorized remote clients.

The following subnet routes have been configured and approved:

| Advertised Subnet | Purpose |
|---|---|
| 10.0.50.0/24 | Proxmox infrastructure |
| 10.0.60.0/24 | Internal storage network |
| 10.0.99.0/24 | Internal network |
| 10.0.100.0/24 | Server infrastructure |
| 10.0.120.0/24 | ZimaBoard and monitoring infrastructure |

Approved subnet routes provide a path to internal networks without requiring Tailscale to be installed on every individual server.

Actual access remains subject to client routing, Tailscale policy, and internal network ACLs.

### Design Benefits

- Encrypted connectivity between enrolled devices
- Remote access without exposing individual management ports
- Centralized subnet routing through the ZimaBoard
- Reduced need for individual VPN clients on infrastructure systems
- Access to multiple internal VLANs through a single routing device

## Cloudflare Tunnel

Cloudflare Tunnel provides externally accessible hostnames for selected homelab web applications.

The tunnel establishes outbound connectivity to Cloudflare, allowing published applications to be reached without traditional inbound firewall port forwarding.

### Published Applications

| Application | Internal Destination | Purpose |
|---|---|---|
| Proxmox VE | 10.0.50.2:8006 | Virtualization management |
| Jellyfin | 10.0.70.2:8096 | Media streaming |
| TrueNAS SCALE | 10.0.60.2 | Storage management |
| Dashy | 10.0.50.5:4000 | Experimental homelab landing page |
| GNS3 | 10.0.55.2:3080 | Network lab management |
| Uptime Kuma | 10.0.120.2:3001 | Service availability status page |
| Apache Guacamole | 10.0.55.100:8080 | Browser-based VNC console access for GNS3 |
| Grafana | 10.0.120.2:3000 | Infrastructure monitoring dashboards |

Eight application routes are configured.

### Application Use Cases

**Infrastructure Management**

Proxmox and TrueNAS web interfaces provide remote access to virtualization and storage management functionality.

**Network Lab Administration**

GNS3 provides a remotely accessible network simulation environment. Apache Guacamole supports browser-based access to VNC consoles associated with the lab.

**Monitoring**

Grafana provides access to infrastructure performance dashboards, while Uptime Kuma provides a status page that family and friends can use to check Jellyfin service availability.

**Media Services**

Jellyfin provides remote media streaming for authorized users.

**Homelab Landing Page**

Dashy was deployed as an experimental centralized landing page. Development is currently paused, and the dashboard is not considered complete.

### Security Architecture

Cloudflare Tunnel reduces the need to expose internal web services through inbound port forwarding.

However, tunnel publishing and application authentication are separate security functions.

Cloudflare Access application policies have not yet been implemented. Published services currently depend on their respective application-level authentication and security configurations.

For example, Grafana uses its native login page for authentication.

Future improvements may include adding Cloudflare Access policies to sensitive management applications.

## AnyDesk

AnyDesk provides remote desktop access to the primary management workstation.

### Deployment

AnyDesk is installed on:

- Personal management laptop
- Mobile phone

### Management Workstation Design

The management laptop is the primary administrative workstation for the homelab.

Restrictive network ACLs limit management connectivity to authorized systems, with the management laptop permitted to access infrastructure that is not broadly accessible from other network segments.

When away from home, AnyDesk allows remote control of the management laptop from the phone.

This enables administration through the existing authorized workstation rather than requiring the phone to have equivalent direct management access to every internal system.

### Administrative Access Workflow

1. Establish an AnyDesk session from the remote device.
2. Connect to the management laptop.
3. Use the laptop's existing management connectivity.
4. Access authorized infrastructure management interfaces.
5. Perform administrative tasks through the established workstation.

This arrangement effectively makes the management laptop a remote administration jump workstation.

### Security Considerations

Because the management laptop has elevated network access, its security is particularly important.

Relevant protections include:

- Strong AnyDesk authentication
- Multifactor authentication where supported
- Restricting unattended remote access
- Keeping the management laptop patched
- Limiting administrative privileges
- Reviewing remote-access permissions
- Maintaining restrictive Cisco network ACLs

A future dedicated travel laptop may provide a more convenient remote administration endpoint.

## Network Segmentation and Access Control

Internal VLAN routing and access control are handled by the Cisco Catalyst 3850 core switch.

The FortiGate provides perimeter security, while the Catalyst switch enforces internal Layer 3 access restrictions.

This separation is important because remote-access connectivity does not automatically imply unrestricted permission to reach every internal system.

### Administrative Access Principles

- Restrict management interfaces to authorized sources
- Maintain separation between user, server, and infrastructure VLANs
- Use a designated management workstation
- Limit access between internal network segments
- Provide remote connectivity without broadly opening internal management services

Tailscale subnet routing, Cloudflare application publishing, and AnyDesk remote control provide different access paths that must be evaluated against these restrictions.

## Operational Use Cases

### Remote Infrastructure Administration

Use AnyDesk to access the management workstation and administer infrastructure through existing ACL-permitted connections.

### Private Network Access

Use Tailscale to establish encrypted connectivity to enrolled devices and approved internal subnets.

### Remote GNS3 Access

Access the GNS3 web interface through its Cloudflare-published hostname and use the Guacamole gateway for browser-based VNC console access.

### Remote Monitoring

Access Grafana dashboards remotely to review infrastructure performance and resource utilization.

### Service Availability Reporting

Provide Jellyfin users with a published Uptime Kuma status page to independently verify service availability.

### Remote Media Streaming

Provide authorized family members and friends with access to the Jellyfin media server through Cloudflare Tunnel.

## Future Improvements

- Evaluate Cloudflare Access policies for sensitive management applications
- Review authentication and authorization across published services
- Evaluate additional Tailscale access restrictions
- Review the security of the AnyDesk management workstation
- Acquire a dedicated travel laptop for remote administration
- Revisit the centralized Dashy landing-page project when higher-priority work is complete
- Document additional access control and security validation procedures

## Technical Skills Demonstrated

**Remote Connectivity:** Tailscale, VPN Subnet Routing, AnyDesk, Cloudflare Tunnel

**Network Security:** Cisco ACLs, VLAN Segmentation, Access Control, Administrative Network Isolation

**Infrastructure Administration:** Proxmox VE, TrueNAS SCALE, GNS3, Apache Guacamole

**Operational Security:** Remote Management, Application Authentication, Reduced Firewall Exposure, Management Workstation Design

**Architecture:** Multi-Method Remote Access, Secure Administration Workflows, Service Publishing, Network Segmentation
