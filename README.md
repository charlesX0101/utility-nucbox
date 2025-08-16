# NucBox Utility Server 

## Overview
This repository documents the build and configuration of a utility server for my homelab environment.
The server, nicknamed utility-nuc, serves as a platform for experimenting with service deployment, VPN technologies, and basic network utilities.  

The purpose of this build is not to create a production service right out the bat, but to establish a **reliable, documented learning environment**. 
Future labs will expand on this foundation, using the services initiated here as building blocks for deeper technical projects.

**Status:** Work in Progress - Base system stack installed and ready for hardware installation and further experimentation.

---

## Project Purpose
- Stand up a utility node separate from core SOC, file, and media servers. 
- Install and configure a small set of services useful for learning and testing:
  - **Syncthing** for file synchronization
  - **Pi-hole** for DNS and network experiments
  - **Tailscale** for identity-based secure remote access
  - **WireGuard** for key-based peer-to-peer VPN practice 
  - Provide a rack-ready base system that can be extended in later labs. 

---

## System Baseline
- **Host Hardware:** NUCBoxG5 (Utility Node) 
- **OS:** Ubuntu Server 24.04.2 LTS (fresh install)
- **Network:** VLAN20, subnet 10.0.20.0/24 
- **Hostname:** `utility-nuc.thebox.lan`
- **Security baseline:**
  - SSH hardened: key-only auth, PAM disabled, MOTD removed
  - Firewall with UFW: default deny inbound, allow outbound
  - LAN exceptions only for required services 

---

## Build Phases

### Phase One: Core OS and Initial Services
- Installed Ubuntu Server 24.04.2 LTS on fresh hardware.
- Integrated into homelab VLAN scheme with static IP. 
- Applied SSH hardening:
  - Disabled password logins
  - Disabled PAM and MOTD
- Configured UFW with default deny, LAN-only rules for management. 
- Installed and validated **Syncthing**:
  - Service enabled at boot
  - Web GUI on port 8384
  - Confirmed accessible from LAN 
- Installed **Pi-hole** but left disabled:
  - Intended for DNS experiments only
  - No DHCP role assigned
  - Service not active in production 

### Phase Two: VPN and Rack-Ready State
- Installed **Tailscale**:
  - Enrolled with identity provider (email login)
  - tailscale0 interface created
  - Tailnet IP assigned
  - SSH verified over tailnet
  - No exit-node or subnet routing enabled
- Installed **WireGuard**:
  - Generated keypair (public/private)
  - Configured `wg0` interface with 10.8.0.1/24
  - Listening on UDP/51820
  - No peers configured (inert, lab-only)
  - Verified with `ip addr` and `wg show`
- Verified firewall posture:
  - No WAN exposure
  - Only Syncthing and Tailscale active
  - WireGuard idle, Pi-hole parked

---

## Rack-Ready Validation
- Node boots clean with services enabled as designed.
- Syncthing GUI reachable only on LAN.
- Tailscale connectivity confirmed through tailnet SSH.
- WireGuard present and listening but safe (no peers). 
- Pi-hole disabled, not impacting network DNS. 
- UFW confirms locked posture with explicit LAN exceptions. 

At this stage, the node can be placed into the homelab rack as a **functional, isolated utility server**.

---

## Deliverables
- Documentation:
  - `docs/utility-node-lab-report.md`
  - Notes and exported configs (`ufw-status.txt`, `wg0.conf` sanitized, `tailscale-status.txt`)
- This `README.md` for project summary

---

## Next Steps
- Future phases will expand WireGuard configuration with peers and controlled exposure. 
- Evaluate Pi-hole for optional production use as a DNS filter. 
- Add centralized logging and monitoring integration with the SOC node. 
- Extend this node’s role in future labs as part of a broader homelab learning ecosystem. 

---

## Notes
This project is deliberately scoped as a **learning-first environment**. 
It is not a final production design, but a step in building confidence with installation, configuration, and security practices across multiple technologies.  
Each service deployed here will be revisited in later labs for deeper dives. 
