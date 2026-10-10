# network-labs

Hands-on networking labs, each documented with its **topology**, **addressing plan**, **device configurations** and **verification steps**.

Built with Cisco Packet Tracer / GNS3.

## Labs

| # | Lab | Topics | Status |
|---|---|---|---|
| 01 | [VLANs & Inter-VLAN Routing](01-vlan-inter-vlan-routing/) | VLAN, 802.1Q trunk, router-on-a-stick, DHCP | ✅ |
| 02 | OSPF single-area | OSPFv2, router ID, passive interfaces | 🔜 |
| 03 | NAT/PAT & ACLs | Static NAT, PAT, standard/extended ACLs | 🔜 |
| 04 | Switch security | Port security, DHCP snooping, SSH-only access | 🔜 |

## Structure

Each lab follows the same layout (see [`_template/`](_template/)):

```
NN-lab-name/
├── README.md        # objectives, topology, addressing, steps, verification
├── configs/         # running-config of each device
└── topology.png     # topology diagram (optional)
```

> All IP addresses and passwords are lab values only.
