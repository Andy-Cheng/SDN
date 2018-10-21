# Ryu Controller Homework2
**Author: Andy Cheng**
## Firewall Task1
![](https://i.imgur.com/fdi6y3o.png)

## Firewall Task2
![](https://i.imgur.com/JkS4ALK.png)

## Discussion and The method used in task2
The key point of finishing task2 is the match fields of flow entries.
There're some key word arguements that we can fill in parser.OFPMatch().
Here's the reference [ryu match fields](https://blog.csdn.net/haimianxiaojie/article/details/50993895?utm_source=blogxgwz11).
``` python=
def switch_features_handler(self, ev):
    #...
    if dpid == 7:
        self.add_flow(datapath, 2, parser.OFPMatch(ip_proto=0x01, ipv4_src='10.0.0.4', ipv4_dst='10.0.0.8', icmpv4_type=8, eth_type=0x800), [])
```
Codes above shows what I modified in simple_switch_13.py taught in Lecture 2.
**icmpv4_type=8** means **ICMP Echo Request** and **icmpv4_type=0** means **ICMP Echo Reply**. ([reference](https://hkitblog.com/%E7%B9%BC%E7%BA%8C-sdn-%E5%AF%A6%E6%88%B0%EF%BC%9A%E5%A6%82%E4%BD%95%E5%88%86%E9%85%8D-flow-%E8%A6%8F%E5%89%87%E5%88%B0%E6%8C%87%E5%AE%9A%E7%9A%84-open-vswitch-%E4%B9%8B%E4%B8%AD%EF%BC%88%E4%B8%8A/))
When icmp packets are sent to host8 from host4, because of higher priority, Switch7 drops them.
On the contrary, host8 still can ping host4, since switch7 can accept icmpv4_type=0 sent from host4 to host8.
Note: If ip_proto is not set, h8 can't ping h4. 0x01 stands for *ICMP*.

