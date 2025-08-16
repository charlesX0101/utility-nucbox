# UFW Status Report – utility-nuc

## Description
This file presents the firewall posture of the utility-nuc server (GMK NucBox G5) at the conclusion of Phase Two. 
UFW (Uncomplicated Firewall) is configured with a default-deny stance, allowing only outbound traffic by default. 
Inbound rules are restricted to specific services and only permitted from the management VLAN (10.0.20.0/24). 
This snapshot demonstrates the rack-ready security state of the node.

## UFW Status Output

```
Status: active
Default: deny (incoming), allow (outgoing), disabled (routed)
Logging: on (low)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    10.0.20.0/24
8384/tcp                   ALLOW IN    10.0.20.0/24
22000/tcp                  ALLOW IN    10.0.20.0/24
22000/udp                  ALLOW IN    10.0.20.0/24
21027/udp                  ALLOW IN    10.0.20.0/24
```
