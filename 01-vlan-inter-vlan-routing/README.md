# 01 · VLANs & Inter-VLAN Routing (Router-on-a-Stick)

## Objectives
- Segment a LAN into three VLANs (Admin, Staff, Guest)
- Configure an 802.1Q trunk between the switch and the router
- Route between VLANs using router subinterfaces
- Serve IP addresses with DHCP on the router
- Restrict Guest VLAN from reaching the Admin VLAN with an ACL

## Topology

```
                 ┌──────────┐
                 │    R1    │
                 └────┬─────┘
                G0/0  │  802.1Q trunk
                G0/1  │
                 ┌────┴─────┐
                 │   SW1    │
                 └┬───┬───┬─┘
            Fa0/1 │   │   │ Fa0/11      Fa0/21
          VLAN 10 │   │ VLAN 20        VLAN 30
              PC-ADMIN PC-STAFF       PC-GUEST
```

## Addressing plan

| VLAN | Name | Subnet | Gateway (R1) | DHCP range |
|---|---|---|---|---|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.1 | .11 – .254 |
| 20 | STAFF | 192.168.20.0/24 | 192.168.20.1 | .11 – .254 |
| 30 | GUEST | 192.168.30.0/24 | 192.168.30.1 | .11 – .254 |
| 99 | MGMT  | 192.168.99.0/24 | 192.168.99.1 | static (SW1 = .2) |

| Switch ports | VLAN |
|---|---|
| Fa0/1 – Fa0/10 | 10 ADMIN |
| Fa0/11 – Fa0/20 | 20 STAFF |
| Fa0/21 – Fa0/24 | 30 GUEST |
| G0/1 | Trunk to R1 (native VLAN 99) |

## Configuration steps
1. Create VLANs 10, 20, 30, 99 on SW1 and name them.
2. Assign access ports to their VLANs and enable PortFast.
3. Configure G0/1 as a trunk with native VLAN 99 and allowed VLANs 10,20,30,99.
4. On R1, create subinterfaces `G0/0.10`, `.20`, `.30`, `.99` with `encapsulation dot1Q`.
5. Configure DHCP pools on R1 and exclude the first 10 addresses of each subnet.
6. Apply an extended ACL inbound on `G0/0.30` to block Guest → Admin.

Full configs: [`configs/R1.txt`](configs/R1.txt) · [`configs/SW1.txt`](configs/SW1.txt)

## Verification

```
SW1# show vlan brief
SW1# show interfaces trunk
R1#  show ip interface brief
R1#  show ip dhcp binding
R1#  show access-lists
```

Expected results:
- PCs receive an address from their VLAN's DHCP pool
- PC-ADMIN ↔ PC-STAFF ping succeeds (inter-VLAN routing works)
- PC-GUEST → PC-ADMIN ping fails, PC-GUEST → PC-STAFF succeeds (ACL works)

## What I learned
- A single physical router link can route multiple VLANs with 802.1Q subinterfaces
- Changing the native VLAN away from VLAN 1 reduces VLAN-hopping risk
- Extended ACLs should be placed as close to the source as possible
