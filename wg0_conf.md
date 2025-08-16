# WireGuard Configuration – utility-nuc

## Description
This file provides the WireGuard configuration for the utility-nuc server (GMK NucBox G5) at the conclusion of Phase Two.
WireGuard was installed, a keypair was generated, and the base interface configuration was created. 
At this stage the interface (wg0) is initialized with a static address and listening port, but no peers have been defined. 
The private key is redacted to preserve security.

## wg0.conf
```
[Interface]
Address = 10.8.0.1/24
ListenPort = 51820
PrivateKey = <REDACTED>
```
# No peers configured at this stage
