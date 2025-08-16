# WireGuard Status Report – utility-nuc

## Description
This file records the WireGuard interface status for the utility-nuc server (GMK NucBox G5) at the conclusion of Phase Two.
The `wg show` command output confirms that the interface (wg0) is active, a keypair has been generated, and the service is listening on the default WireGuard port (51820/udp).  
No peers are defined at this stage, which means the interface is initialized but inert, matching the rack-ready objective for this phase.

## wg show Output

```
interface: wg0
  public key: 0TTrz/IoinFdwNZe9WR0YdmvvtW7zp9QhSPh6cqRcGE=
  private key: (hidden)
  listening port: 51820
  peers: (none)
```
