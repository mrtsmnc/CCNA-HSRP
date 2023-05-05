# CCNA-HSRP

When a problem occurs with a router in a network, the continuity of routing is ensured through redundancy protocols. HSRP (Hot-Standby Router Protocol) by Cisco is one of these protocols. Like VRRP (Virtual Router Redundancy Protocol), HSRP allows multiple routers to act as a single virtual router. HSRP provides redundancy by minimizing the unavailability time of the central router in milliseconds.

***The main differences between HSRP and the other redundancy protocol, VRRP, are as follows:

**While HSRP is a Cisco proprietary protocol, VRRP is an open protocol defined by IEEE standards.
**In HSRP, the IP and MAC addresses of the virtual router are not the same as the actual IP and MAC addresses of any router in the network, whereas there is no such requirement in VRRP.
**In VRRP, the router that has the IP address of the virtual router becomes the primary router.
**While HSRP has only one standby router, VRRP can have multiple standby routers.

Routers running HSRP assign themselves a virtual IP (Internet Protocol) and MAC (Media Access Control) address. These addresses serve as the gateway for clients. Even if the exit routers become unreachable and start using another router to access the internet, the IP and MAC addresses of the gateway do not change, so there is no problem with internet access. HSRP operates over UDP (User Datagram Protocol) on port 1985.

An HSRP group number is assigned to the appropriate interfaces or VLAN interfaces of routers running HSRP. The following command selects the virtual gateway IP address assigned for the HSRP group.

***Router(config-if)#standby [group no] ip [virtual gateway ip address]

Among routers running HSRP, the router with the highest priority value is active, and the router with the next highest priority value is in standby mode. The remaining routers listen to HSRP status and exchange HSRP Hello packets at regular intervals. If the priority values of all routers in the topology are equal, the interface with the highest IP address becomes the active side of that HSRP group. The priority value can be changed with the following command. By default, this value is 100. The maximum value that can be assigned is 255.

**Router(config-if)#standby [group no] priority [priority value]

The active router is the main router that performs routing in the network. The standby router receives Hello packets from the active router every 3 seconds. This time period is called hold-time. If the standby router fails to receive Hello packets within this hold-time, it takes over the active role. The destination IP address of the Hello packets is a multicast IP address of 224.0.0.2, which means that the packet will be sent to all routers in the network. To change the hold-timer between routers, the following command is used:

**Router(config-if)#standby [group no] timers [hello interval] [standby delay time]

If a backup router takes over as the new active router, the previous active router will become the backup router and cannot become active again unless the following command is entered. However, if the new backup router also fails, and there is no active router on the network, the previous active router can become active again.

**Router(config-if)#standby [group no] preempt

When an active router goes down, the clients in the network lose their connection to the network. HSRP detects this situation and allows other routers to become active. It does this by lowering the priority value of the current active router. This situation continues until at least one reachable path remains in the topology. To lower the "Priority" value, the following command is used:

***Router(config-if)#standby [group no] authentication 

Within a network, all routers belonging to the same HSRP group listen to ARP (Address Resolution Protocol) requests and build ARP tables. However, only the active router responds to ARP requests even if the request is initiated by the standby router. In an HSRP topology, HSRP routers cannot initiate communication using the virtual gateway IP address. For example, a 'ping' cannot be sent to any address in the topology using the virtual gateway IP address as the source. To provide authentication to HSRP packets, the following command is used:
