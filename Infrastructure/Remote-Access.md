# Remote Access

## Overview

This document describes the remote access solutions used within the HuntWired homelab.

## Tailscale

### Purpose

Provides secure remote access to internal infrastructure without exposing services directly to the Internet.

### Features

- Secure Remote Administration
- Remote RDP Access
- Access to Internal Services
- Encrypted Connectivity

### Managed Systems

- Proxmox
- TrueNAS
- Domain Controllers
- Veeam
- Jellyfin
- ZimaBoard

## Cloudflare Tunnel

### Purpose

Provides secure external access to selected services.

### Published Services

- Jellyfin
- Uptime Kuma Status Page

### Benefits

- No Open Firewall Ports
- Secure HTTPS Access
- Reduced External Attack Surface

## Security Considerations

- Zero Trust Access Model
- Encrypted Communications
- Limited Public Exposure
- Management Access Restricted to Authorized Devices

## Future Improvements

- Additional Tailscale ACL Policies
- Expanded Zero Trust Controls
- Additional Service Publishing
