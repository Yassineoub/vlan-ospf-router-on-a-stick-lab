===========================================================
 Enterprise Network Lab: VLANs, Router-on-a-Stick & OSPF
 Tool: Cisco Packet Tracer
===========================================================

OVERVIEW
--------
A small enterprise network designed and built in Cisco Packet Tracer.
One address block (192.168.1.0/24) is divided with VLSM into four
departments. Each department sits in its own VLAN, and a single router
provides inter-VLAN routing (Router-on-a-Stick) and connects to a
server network. OSPF is configured on the router.

TOPOLOGY
--------
- R1 (router) -> SW0 (core switch) -> SW1, SW2, SW3, SW4 (department switches)
- R1 -> SW5 -> Server0 (server network, 172.16.1.0/30)
- Department switches connect to SW0 through trunk links.

REQUIREMENTS
------------
Department   Devices needed
HR           34
IT           50
Sales        25
Accounting   10

STEP 1 - SUBNETTING (VLSM)
--------------------------
Base network: 192.168.1.0/24
Subnets are allocated from the largest to the smallest to avoid waste.

Dept        Needed  Subnet            Mask             Usable range                   Broadcast
IT          50      192.168.1.0/26    255.255.255.192  192.168.1.1   - 192.168.1.62   192.168.1.63
HR          34      192.168.1.64/26   255.255.255.192  192.168.1.65  - 192.168.1.126  192.168.1.127
Sales       25      192.168.1.128/27  255.255.255.224  192.168.1.129 - 192.168.1.158  192.168.1.159
Accounting  10      192.168.1.160/28  255.255.255.240  192.168.1.161 - 192.168.1.174  192.168.1.175

Router-to-server link: 172.16.1.0/30 (2 usable addresses, point-to-point).

STEP 2 - VLANs
--------------
One VLAN per department, created on the switches. Ports connected to
PCs are access ports in the correct VLAN. Links between the switches
and SW0, and between SW0 and the router, are trunk ports.

Dept        VLAN ID
IT          10
HR          20
Sales       30
Accounting  40

STEP 3 - INTER-VLAN ROUTING (ROUTER-ON-A-STICK)
-----------------------------------------------
R1 has one physical interface connected to SW0 (trunk). On it, one
sub-interface is created per VLAN with 802.1Q encapsulation. Each
sub-interface uses the first usable address of its subnet and acts as
the default gateway for that department.

Sub-interface  VLAN  IP address / mask   Role
Gi0/0.10       10    192.168.1.1/26      IT gateway
Gi0/0.20       20    192.168.1.65/26     HR gateway
Gi0/0.30       30    192.168.1.129/27    Sales gateway
Gi0/0.40       40    192.168.1.161/28    Accounting gateway

STEP 4 - ROUTER INTERFACE TO THE SERVER NETWORK
-----------------------------------------------
The router interface toward SW5 uses the 172.16.1.0/30 network:
- R1:      172.16.1.1
- Server0: 172.16.1.2

STEP 5 - OSPF
-------------
OSPF was enabled on R1 (area 0) and the department networks and the
172.16.1.0/30 network were advertised.

STEP 6 - STATIC IP ADDRESSING
-----------------------------
All PCs and the server were given static IP addresses, subnet masks and
default gateways according to their department subnet.

VERIFICATION
------------
- Ping between PCs in the same VLAN
- Ping between PCs in different VLANs (routed through R1)
- Ping from PCs to Server0 (172.16.1.2)
- show vlan brief, show ip route, show ip ospf

SKILLS DEMONSTRATED
-------------------
VLSM subnetting, VLANs and trunking, Router-on-a-Stick, OSPF,
static IP addressing, network troubleshooting

FILES IN THIS REPOSITORY
------------------------
- lab.pkt        Packet Tracer project file
- topology.png   Screenshot of the topology
- configs.txt    Router and switch configurations
