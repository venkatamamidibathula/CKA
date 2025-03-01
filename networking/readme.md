**Networking Basics**

- For two systems to communicate with each other we need a switch.
**ip link** to see the interfaces for each systems
- For systems to connect to switch we need a network interface card. (Ethernet card)
**ip addr add 192.168.1.10/24 dev eth0**
**ip addr add 192.168.1.11/24 dev eth0**


- now both systems can ping each other in a network.

Between two networks , a router helps connect to seperate networks togethers.

execute **route** command to display kernel routing table.

To access network 192.168.2.0/24 add routing table.

ip route add 192.168.2.0/24 via 192.168.1.1

ip route add default via 192.168.2.1.








