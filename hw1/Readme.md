# Mininet and Ryu Basic Lab HW1
**Author: Andy Cheng**
## Q1-1: Please explain why we need to set up the arp table for each host?
- Ans: 
    Arp table is used for mapping from IP address to physical address of a host. Each row in the arp table is dymnamic, which means each of them has its life span, and is constructed when a host is trying to send a packet to another host but doesn't know it's phisical address mapped by a given IP. If we don't construct the arp table, during run time, host can't resolve other's physical address and thus a packet can't reach its destination.
## Q1-2: If the arp table is not set up, what will happen in the network? 
- Ans:
    If the arp table is not set yet, the host won't resolve the physical destination.
    Ex. **grid3x3.py**
    + The arp table has'nt been set before h1 pings h3.
        ![](https://i.imgur.com/3VrgjCy.png)
        **packets are unreachable**
    + The arp table has been set before h1 pings h3.
        ![](https://i.imgur.com/ssxGy5L.png)
        **packets are reachable**
    
## Q1-3 For different topologies, do we always need to set up the arp table? Please explain your reason.
- Ans: No. If the topology are not closed loop, by ARP query (broadcast), the arp table can be established during runtime. For example, there's only a switch and two hosts in the topology. 
    + Below is the scneario. 
    1. Host 1 doesn't know the Mac address of 192.168.1.1 and it broadcasts a message to query that Mac. Everyone in the local network receive that message.
    ![](https://i.imgur.com/5BkYk50.png =500x)
    2. The host with that IP responds to that message, and Host 1 writes the IP-Mac mapping into the arp table.
    ![](https://i.imgur.com/CHut58I.png =500x)
    On the contrary, If the loop is closed, whenever broadcasting, broadcast storm will occur.
    Broadcast storm: When a server sends an Arp query, Switch A receives this message, and floods out this message.
    Switch B then receives this message, and also floods out this message. Subsequently, Switch A receives this message from B, and it turns out to be an infinite loop. 
    
## Q1-4 Is there any algorithm to avoid the problem? 
- Ans: Yes, the problem can be solved by loop avoidance.
    STP(Spanning Tree Protocol) is a protocol solveing broadcast storm and one of the algoreithms for loop avoidance.
    Below is the procedure of STP.
    BDUP: Bridge Protocol Data Unit
    1. Root bridge selection
        After exchanging BDUP between switches, the switch with the smallest ID is Root. After this step, Root will send the original BDUP, and other switches will receive and forward BDUP.
    2. Port role decision
    The Roles of ports are decided by distance from Root to ports.
         + Root port
         + Designated port
         + Non designated port
         Non designated ports can't transmit frames.
         ![](https://i.imgur.com/dcfrrwX.png =300x)

    3. State changing of ports
    After the roles of ports have been decided, every ports will stay at the state of LISTEN, and latter will fall into state transitoin described below.
    ![](https://i.imgur.com/dCnWZ9L.png =600x)
    ![](https://i.imgur.com/Q5RPmNY.png =300x)
    
## Q2 (wireshark)
#### Using filter
![](https://i.imgur.com/3xBkzsa.png =800x)
#### Packet meta and content
![](https://i.imgur.com/riP72To.png =800x)
#### Packet TCP stream
![](https://i.imgur.com/p8WCuRV.png =400x)
