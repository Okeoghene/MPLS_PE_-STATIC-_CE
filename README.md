# MPLS PE <-STATIC-> CE Network

## Overview

This project defines a **Multiprotocol Label Switching (MPLS)** Provider Edge (PE) and Customer Edge (CE) network configuration with static routing between PE and CE devices. It consists of multiple routers interconnected using Ethernet interfaces, loopback interfaces, and OSPF for internal routing within the provider network.

## Network Topology

The topology consists of the following devices:

- **Customer Edge Routers (CE)**
  - `Cust-Edge1`
  - `Cust-Edge2`
- **Provider Edge Routers (PE)**
  - `Prov-Edge1`
  - `Prov-Edge2`
- **Provider Router (P)**
  - `Prov`

## IP Addressing Schema

### Router Interface IP Addressing

| Device     | Interface | Connected To    | IP Address     |
| ---------- | --------- | --------------- | -------------- |
| Cust-Edge1 | Loopback0 | Cust-Edge1      | 11.11.11.11/32 |
| Cust-Edge1 | E0/0      | Prov-Edge1 E0/0 | 192.168.1.0/31 |
| Prov-Edge1 | E0/0      | Cust-Edge1 E0/0 | 192.168.1.1/31 |
| Prov-Edge1 | E0/1      | Prov E0/1       | 192.168.1.2/31 |
| Prov       | E0/1      | Prov-Edge1 E0/1 | 192.168.1.3/31 |
| Prov       | E0/2      | Prov-Edge2 E0/2 | 192.168.1.4/31 |
| Prov-Edge2 | E0/2      | Prov E0/2       | 192.168.1.5/31 |
| Prov-Edge2 | E0/0      | Cust-Edge2 E0/0 | 192.168.1.6/31 |
| Cust-Edge2 | E0/0      | Prov-Edge2 E0/0 | 192.168.1.7/31 |
| Cust-Edge2 | Loopback0 | Cust-Edge2      | 22.22.22.22/32 |

### Loopback IP Addresses

| Device     | Interface | IP Address     |
| ---------- | --------- | -------------- |
| Cust-Edge1 | Loopback0 | 11.11.11.11/32 |
| Prov-Edge1 | Loopback0 | 1.1.1.1/32     |
| Prov       | Loopback0 | 2.2.2.2/32     |
| Prov-Edge2 | Loopback0 | 3.3.3.3/32     |
| Cust-Edge2 | Loopback0 | 22.22.22.22/32 |

## Configuration Details

Each router is configured with the necessary static routes and OSPF for internal communication within the provider network.

### OSPF and BGP Configuration

- **OSPF is used** between provider routers (`Prov`, `Prov-Edge1`, and `Prov-Edge2`).
- **BGP is used** between provider edge routers to exchange VPNv4 routes.
- Static routes are defined on `Cust-Edge1` and `Cust-Edge2` to reach the provider network.

### VRF Implementation

- The provider edge routers (`Prov-Edge1`, `Prov-Edge2`) use VRF **super** with Route Distinguisher (RD) **111:1** and Route Targets **111:1** for importing/exporting VPN routes.

## Running the Configuration

To apply these configurations in a Cisco virtual lab environment:

1. Load the YAML file into **GNS3** or **EVE-NG**.
2. Start the network devices and verify interface status.
3. Check OSPF and BGP neighbors:
   ```bash
   show ip ospf neighbor
   show bgp vpnv4 unicast summary
   ```
4. Validate static routes on customer edge routers:
   ```bash
   show ip route
   ```
5. Test connectivity using `ping` and `traceroute` commands.

## Troubleshooting

- Ensure correct interface assignments and IP configurations.
- Check OSPF neighbor status using:
  ```bash
  show ip ospf neighbor
  ```
- Verify MPLS label bindings:
  ```bash
  show mpls ldp bindings
  ```
- Confirm BGP VPNv4 route exchange:
  ```bash
  show bgp vpnv4 unicast summary
  ```

## Conclusion

This YAML file represents a **static routing-based MPLS PE-CE network** with **OSPF, BGP, and VRF configuration** for VPN services. It can be used for testing MPLS-based service provider scenarios in a lab environment.

