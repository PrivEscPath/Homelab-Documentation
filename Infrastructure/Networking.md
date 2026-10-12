# Networking

## Overview

The HuntWired homelab uses a segmented network architecture built around a Cisco Catalyst 3850 Layer 3 switch and a FortiGate perimeter firewall.

The network is designed to support enterprise infrastructure, virtualization, security testing, and everyday household devices while providing opportunities to develop practical networking and security engineering skills.

## Network Architecture

### Cisco Catalyst 3850 — Layer 3 Core

The Cisco Catalyst 3850 serves as the primary network core and is responsible for:

- Inter-VLAN routing using switched virtual interfaces (SVIs)
- ACL-based traffic filtering between network segments
- Network segmentation and access restrictions
- LACP EtherChannel connectivity to infrastructure devices
- Dedicated infrastructure and management VLANs

**Design Decision:**

Inter-VLAN routing and network access controls were intentionally implemented on the physical Cisco switch rather than the FortiGate.

Although FortiGate supports these capabilities, implementing them directly on the Catalyst 3850 provides hands-on experience with Cisco Layer 3 switching, routing, ACL development, and troubleshooting.

This architecture also separates internal network routing and segmentation from perimeter firewall responsibilities.

### FortiGate — Perimeter Firewall

The FortiGate provides internet-edge connectivity and perimeter security.

Its responsibilities include:

- Internet-facing firewall policies
- NAT and outbound internet connectivity
- Static routing between the firewall and internal network
- Perimeter traffic inspection and security enforcement

The FortiGate is connected to the Cisco core through an LACP port channel.

Internal inter-VLAN routing and segmentation are performed by the Catalyst 3850 rather than the FortiGate.

## VLAN Architecture

VLANs separate infrastructure services, management interfaces, testing environments, and household devices.

| VLAN | Purpose |
|---|---|
| 10 | Primary Wi-Fi |
| 20 | Secondary / IoT Wi-Fi |
| 30 | Guest Wi-Fi / Alexa |
| 40 | Televisions |
| 50 | Proxmox host management and Cloudflared |
| 51 | Proxmox Windows testing environment |
| 52–54 | Reserved Proxmox lab networks |
| 55 | GNS3 network simulation environment |
| 60 | TrueNAS storage infrastructure |
| 70 | Jellyfin media infrastructure |
| 80 | Gaming console |
| 90 | Home PCs and laptops |
| 100 | Administrative infrastructure, Active Directory, and Veeam |
| 120 | ZimaBoard infrastructure |
| 999 | Network management |

### Planned Network Expansion

VLAN 110 is reserved for a future shared printer network to provide printer access to authorized household devices.

## Network Security

Security controls are primarily enforced through the Cisco Catalyst 3850.

Key architectural controls include:

- ACL-based restrictions between VLANs
- Dedicated management network
- Separation of household devices from infrastructure systems
- Segmented virtualization and testing networks
- Restricted access to administrative resources

The GNS3 environment uses a dedicated VLAN to separate simulated networking and security-testing workloads from other infrastructure.

## Link Aggregation

LACP EtherChannels provide aggregated connectivity between the Cisco core and supported infrastructure devices.

Current infrastructure includes port channels for the FortiGate and virtualization-related systems.

Detailed interface assignments, configurations, and verification procedures will be maintained in dedicated technical repositories.

## Network Services

The environment supports:

- DHCP
- DNS
- Layer 3 routing
- VLAN segmentation
- Infrastructure management connectivity

## Planned Improvements

- Review and harden the native VLAN configuration on the FortiGate trunk.
- Implement the dedicated printer VLAN.
- Continue reviewing VLAN assignments and ACL effectiveness.
- Expand technical documentation of network configuration, validation, and troubleshooting.

## Related Documentation

Detailed Cisco configurations, ACL implementation, routing design, and troubleshooting procedures will be maintained in a dedicated Network Engineering repository.

Network topology diagrams are maintained in the Homelab-Documentation repository.
