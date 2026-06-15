# Networking

## Overview

This document describes the network architecture of the HuntWired homelab.

## Core Infrastructure

### Cisco Catalyst 3850

- Layer 3 Routing
- Inter-VLAN Routing
- LACP Port Channels
- ACL-Based Segmentation
- Management VLAN Isolation

### FortiGate Firewall

- Internet Edge Firewall
- Static Routing
- Policy Based Routing
- VLAN Connectivity

## VLAN Architecture

| VLAN | Purpose |
|--------|----------|
| 10 | Main WiFi |
| 20 | IoT / Son Devices |
| 30 | Guest |
| 40 | TVs |
| 50 | Infrastructure |
| 60 | Storage |
| 70 | Jellyfin |
| 90 | Administration |
| 100 | Active Directory |
| 999 | Management |

## Security Controls

- ACL-Based Network Segmentation
- Restricted Management Access
- Dedicated Management VLAN
- Inter-VLAN Traffic Controls

## Network Services

- DHCP
- DNS
- Routing
- VLAN Segmentation
