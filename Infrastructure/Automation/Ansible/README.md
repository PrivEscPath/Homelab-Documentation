# Ansible Automation

## Overview

This project documents the ongoing development of Ansible automation within the HuntWired homelab.

Ansible is intended to simplify infrastructure administration, reduce repetitive tasks, and improve configuration consistency across managed systems.

**Project Status: Work in Progress**

## Deployment

| Component | Configuration |
|---|---|
| Automation Platform | Ansible |
| Host | ZimaBoard 832 |
| Operating System | Debian 13 |
| Deployment | Docker container |
| Previous Deployment | Proxmox LXC |

The original Ansible LXC was retired during major network and virtualization infrastructure changes.

Ansible has since been redeployed as a Docker container on the ZimaBoard. The current automation configuration and managed host inventory have not yet been fully validated.

## Repository Contents

- `Inventory` — Legacy Ansible host inventory requiring review
- `Playbooks/` — Existing automation playbooks from the previous deployment

These files are retained for reference and may require updates to reflect the current infrastructure.

## Future Development

- Validate the current Ansible deployment
- Rebuild and verify the managed host inventory
- Review and update existing playbooks
- Develop reusable Linux maintenance automation
- Explore Proxmox administration through Ansible
- Implement secure authentication and configuration management
- Document and test completed automation workflows

## Technical Skills

Ansible, Docker, Debian Linux, YAML, Infrastructure Automation, Configuration Management, Proxmox VE
