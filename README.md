#  RIPv2-Enterprise-Branch-Network

A small enterprise network designed and implemented in **Cisco Packet Tracer** to demonstrate RIPv2 dynamic routing, VLSM-based IP addressing, passive interfaces, server connectivity, route verification, and network troubleshooting.

The project represents a company with multiple offices connected through routed WAN links. Each branch has its own LAN, while RIPv2 is used to exchange routes between the routers.

## Project Overview

The network is designed around a simple enterprise branch-office scenario:

* Chattogram Head Office
* Dhaka Branch Office
* Cumilla Branch Office
* Additional router for the Server network
* Two servers for internal network services
* Separate LAN and WAN networks
* RIPv2 for dynamic route exchange
* VLSM-based IPv4 addressing
* Passive interfaces on user-facing LANs
* End-to-end connectivity and troubleshooting

The main goal is to build a working routed network and understand how routers learn and forward traffic dynamically without manually configuring every remote route.

---

## Network Topology

```text
                         R2
                    Dhaka Branch
                    /          \
                   /            \
                  /              \
        R1 ---------------------- R3
   Chattogram HO              Cumilla Branch
        |                          |
       SW1                        SW3
     /    \                     /    \
   PC1    PC2                  PC5    PC6

                         |
                    Additional Router
                         |
                    Server Network
                    /           \
               Server1        Server2
```

> The topology may vary slightly depending on the Packet Tracer router model and available interfaces.

---

## Network Design

The network uses separate IP networks for each office and point-to-point WAN connection.

### LAN Networks

| Location               | Network           | Prefix | Gateway        |
| ---------------------- | ----------------- | -----: | -------------- |
| Chattogram Head Office | `192.168.10.0/26` |    /26 | `192.168.10.1` |
| Dhaka Branch           | `192.168.30.0/27` |    /27 | `192.168.30.1` |
| Cumilla Branch         | `192.168.20.0/28` |    /28 | `192.168.20.1` |
| Server                 | `192.168.40.0/28` |    /28 | `192.168.40.1` |

### WAN Networks

| Link    | Network        | Router A    | Router B    |
| ------- | -------------- | ----------- | ----------- |
| R1 – R2 | `10.0.22.0/30` | `10.0.22.1` | `10.0.22.2` |
| R2 – R3 | `10.0.23.0/30` | `10.0.23.1` | `10.0.23.2` |
| R3 – R4 | `10.0.24.0/30` | `10.0.24.1` | `10.0.24.2` |

### Server Network

The additional router provides connectivity to a dedicated server network containing two internal servers.

```text
Server Network
      |
      +---- Server1
      |
      +---- Server2
      |
Additional Router
```

The server IP addressing is documented in the project configuration files and Packet Tracer topology.

---

## Technologies and Concepts

* Cisco IOS
* Cisco Packet Tracer
* IPv4
* VLSM
* RIPv2
* Dynamic Routing
* Static IP Addressing
* Passive Interfaces
* LAN and WAN Design
* Routing Table Analysis
* Basic Network Troubleshooting
* Server Connectivity
* End-to-End Connectivity Testing

---

## RIPv2 Configuration

RIPv2 is configured on the routers to exchange routes dynamically.

Basic configuration:

```cisco
router rip
 version 2
 no auto-summary
 network 192.168.10.0
 network 10.0.12.0
```

The exact `network` statements differ between routers according to their connected networks.

### Why RIPv2?

RIPv2 was selected for this project because it provides a straightforward way to demonstrate dynamic routing and route exchange.

It also supports:

* Classless routing
* VLSM
* Subnet mask advertisement
* Route summarization control
* RIP authentication
* Multicast updates

RIPv2 is mainly used here as a learning and demonstration protocol. In larger modern enterprise networks, protocols such as OSPF are generally more appropriate.

---

## Passive Interface

User-facing LAN interfaces are configured as passive interfaces.

Example:

```cisco
router rip
 passive-interface g0/0
```

The purpose is to prevent unnecessary RIP updates from being sent toward end devices while still allowing the connected network to be advertised through RIP.

WAN interfaces between routers remain active so that routing updates can be exchanged.

---

## VLSM Addressing

Different subnet sizes are used for the branch networks:

```text
Chattogram    192.168.10.0/26
Dhaka         192.168.30.0/28
Cumilla       192.168.20.0/27
Server        192.168.20.0/29
```

This demonstrates the use of **Variable Length Subnet Masking (VLSM)** instead of assigning the same subnet size to every LAN.

VLSM allows IP address space to be allocated according to the requirements of each network.

---

## Verification

The following Cisco IOS commands were used to verify the network:

```cisco
show ip interface brief
```

Used to verify interface status and IP addressing.

```cisco
show ip protocols
```

Used to verify the RIPv2 process, advertised networks, and passive interfaces.

```cisco
show ip route
```

Used to examine the complete routing table.

```cisco
show ip route rip
```

Used to display routes learned through RIP.

```cisco
show running-config | section router rip
```

Used to review the RIPv2 configuration.

### Connectivity Testing

End-to-end connectivity was tested using ICMP:

```text
PC1 → Remote Branch PC
PC1 → Server1
PC1 → Server2
PC5 → Chattogram LAN
Server1 → Remote Branch
```

Successful ping tests confirm that the routers are learning the required remote networks and forwarding packets correctly.

---

## Troubleshooting

Several common routing and connectivity problems were tested during the project.

### Interface Failure

```cisco
show ip interface brief
```

Used to identify interfaces that are down or administratively shut down.

### Missing RIP Route

```cisco
show ip route rip
show ip protocols
```

Used to determine whether a remote network is being advertised and learned correctly.

### WAN Connectivity

```cisco
ping 10.0.22.2
ping 10.0.23.2
```

Used to verify connectivity between neighboring routers.

### End-to-End Connectivity

```cisco
ping 192.168.30.10
```

Used to verify communication between different branch networks.

---
 

## Project Outcome

This project provided practical experience with:

* Designing a multi-branch routed network
* Planning IPv4 addressing with VLSM
* Configuring Cisco router interfaces
* Implementing RIPv2 dynamic routing
* Using passive interfaces
* Reading and interpreting routing tables
* Understanding RIP hop count and administrative distance
* Testing end-to-end network connectivity
* Troubleshooting routing and interface failures
* Connecting internal servers to a routed network
* Documenting a network project for professional use

---

## Packet Tracer File

The complete Cisco Packet Tracer project is included in this repository:

```text
packet-tracer/enterprise-ripv2.pkt
```

Open the file with Cisco Packet Tracer to inspect the topology, configurations, IP addressing, routing tables, and connectivity tests.

---
## Packet Tracer command

Dhaka Brance >>

ena	
conf t
host Dhaka_Brance_Router
int gi 0/1
no shut
ip add 192.168.30.1 255.255.255.240
ex
int gi 0/0
no shut
ip add 10.0.22.1 255.255.255.252
exit
router rip
version 2
network 10.0.22.0
network 192.168.30.0
no auto-summary
end
copy run start


 

Cumilla Brance >>

ena	
conf t
host Cumilla_Brance_Router
int gi 0/2
no shut
ip add 192.168.20.1 255.255.255.224
ex
int gi 0/0
no shut
ip add 10.0.22.2 255.255.255.252
exit
int gi 0/1
no shut
ip add 10.0.23.1 255.255.255.252
exit
router rip
version 2
network 10.0.22.0
network 10.0.23.0
network 192.168.20.0
no auto-summary
end
copy run start


Chattoggram Head office Brance >>

ena	
conf t
host Chattoggram_Head_Office_Brance_Router
int gi 0/1
no shut
ip add 192.168.10.1 255.255.255.192
ex
int gi 0/0
no shut
ip add 10.0.23.2 255.255.255.252
exit
int gi 0/2
no shut
ip add 10.0.24.1 255.255.255.252
exit
router rip
version 2
network 10.0.24.0
network 10.0.23.0
network 192.168.10.0
no auto-summary
end
copy run start


Server >>

ena	
conf t
host Server_Router
int gi 0/0
no shut
ip add 192.168.40.1 255.255.255.248
ex
int gi 0/1
no shut
ip add 10.0.24.2 255.255.255.252
exit
router rip
version 2
network 10.0.24.0
network 192.168.40.0
no auto-summary
end
copy run start


## Author

**Mohammed Raihan**

CSE Graduate
CCNA • MTCNA • Networking • Linux • Cybersecurity

---

## Disclaimer

This project was created for networking practice and portfolio development using Cisco Packet Tracer. RIPv2 is used to demonstrate dynamic routing fundamentals and is not intended to represent a recommended routing protocol for every production enterprise environment.
