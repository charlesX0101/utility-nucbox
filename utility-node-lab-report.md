# Utility NUC Server – Lab Report (utility-nuc)
---

## 1. Project Context

This lab documents the complete lifecycle of the Utility NUC Server, referred to as *utility-nuc*. 
The node was introduced into the homelab to act as a versatile testbed for essential services:
- File synchronization (Syncthing)
- DNS experimentation (Pi-hole)
- Identity-based remote access (Tailscale)
- Peer-to-peer VPN training (WireGuard)

Unlike production servers in the environment (data server, media node, etc.), this machine is deliberately scoped as an **educational platform**. It is rack-ready, secured, and lightly provisioned, leaving space for iterative configuration in future labs. These services will be brought to production once they are dialed in and configured properly. Since my homelab is a full, in production working system, we cannot jump straight to full production of these services just yet but will do so in future labs. 

---

## 2. System Baseline

**Hardware:** GMK NucBox G5 (Utility Node) 
**Operating System:** Ubuntu Server 24.04.2 LTS (clean install) 
**Network:** VLAN20, subnet 10.0.20.0/24 
**Hostname:** utility-nuc 
**IP Address:** 10.0.20.22 (static) 

**Security hardening:**
- SSH key-only authentication
- PAM disabled
- MOTD removed
- UFW configured:
  - Default deny inbound, allow outbound
  - Explicit LAN rules for Syncthing and SSH

---

## 3. Phase One – Initial Build

**OS and Network Integration**
- Installed Ubuntu Server 24.04.2 LTS from ISO
- Set hostname and static IP within VLAN20
- Confirmed gateway/DNS reachability

**SSH Hardening**
- Disabled password authentication
- Disabled PAM modules to simplify login flow
- Removed default MOTD and banner clutter
- Enabled key-only login

**Firewall**
- Installed and configured UFW
- Policy: deny incoming, allow outgoing
- Explicit allow rules for:
  - SSH from 10.0.20.0/24
  - Syncthing ports from 10.0.20.0/24

**Services**
- **Syncthing**
  - Installed via APT
  - Configured as systemd service
  - Web GUI bound to port 8384
  - Verified accessible at http://10.0.20.x:8384
- **Pi-hole**
  - Installed successfully
  - Service disabled (not active in production)
  - Reserved for lab-only DNS experimentation

---

## 4. Phase Two – VPN Integration

**Tailscale**
- Installed with official script
- Joined tailnet using email identity
- tailscale0 interface created
- Tailnet IP assigned (100.x.x.x)
- Verified connectivity via `tailscale status` and `tailscale ip`
- Tested tailnet SSH from workstation
- No exit-node or subnet-router duties enabled

**WireGuard**
- Installed via APT
- Keypair generated:
  - Public key: `0TTrz/IoinCdwNZe9WR0YdmvvtW7zp9NhSPh6cqRcGE=`
  - Private key stored securely at `/etc/wireguard/privatekey`
- Configured `/etc/wireguard/wg0.conf` with:
  - Interface address: 10.8.0.1/24
  - Listen port: 51820/udp
  - No peers defined (safe, inert state)
- Enabled and verified with:
  - `ip addr show wg0`
  - `sudo wg show`
- UFW left closed to WAN; no NAT or routing enabled

---

## 5. Validation and Verification

**SSH**
- Accessible from VLAN20 and tailnet
- Denied from non-permitted VLANs

**Syncthing**
- GUI reachable at 10.0.20.x:8384 (LAN only)
- File synchronization functional

**Pi-hole**
- Service installed but inactive
- Confirmed disabled at boot

**Tailscale**
- Node visible in admin console
- Tailnet IP reachable from other peers
- Verified `tailscale ping` and tailnet SSH

**WireGuard**
- wg0 interface present and assigned 10.8.0.1/24
- Keys confirmed
- No peers listed (inert until future labs)

**Firewall (UFW)**
- Default deny inbound
- Explicit LAN-only rules applied
- Confirmed no unexpected exposure with `ss -tulwn`

---

## 6. Deliverables

This repository contains:
- `README.md` – Project overview and summary
- `utility-node-lab-report.md` – This detailed report
- `ufw-status.txt` – UFW rule snapshot
- `tailscale-status.txt` – Tailscale peer list (sanitized)
- `wg0.conf` – WireGuard config (keys redacted)
- `wg-show.txt` – WireGuard status output

---

## 7. Lessons Learned

- Scope discipline is essential: this project nearly grew into a mini SOC box.
- Tailscale provides immediate, low-friction remote access; WireGuard teaches fundamentals. Running both side-by-side is valuable for comparative learning.  
- Installing Pi-hole in a lab but keeping it inactive is a safe way to prevent unintended production disruption.
- Documenting firewall state and service roles ensures clarity when revisiting the node later.
- Continued learning how important each server and core service works together to make the network come alive. 

---

## 8. Next Steps

- Expand WireGuard with peers for lab-only connectivity tests.
- Evaluate Pi-hole for DNS filter role in non-critical VLAN. 
- Introduce centralized logging (rsyslog forwarding to SOC node).
- Consider lightweight monitoring agent for metrics collection.
- Build new labs on top of this server to practice controlled service deployments.

---

## 9. Conclusion

The Utility NUC Server (*utility-nuc*, GMK NucBox G5) is now **rack-ready** with four services installed, two active, one idle, and one parked.  
It provides a safe, secure environment to practice VPN, synchronization, and DNS concepts without impacting critical homelab infrastructure.  
This lab represents a foundation: future projects will build on this node, deepening the learning environment step by step.
