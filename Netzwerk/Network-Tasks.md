
## LAG verstehen und aktivieren  
LAG, LACP  

https://administrator.de/tutorial/link-aggregation-lag-im-netzwerk-1586053518.html  

## Netzwerkverkehr visualisieren  

https://administrator.de/tutorial/netzwerkverkehr-mit-netflow-bzw-ipfix-visualisieren-3915529709.html


## IP herausfinden (ungetestet)

### am l3 core
sh ip arp

Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.8.0.10               7   0008.5d8d.5634  ARPA   Vlan8
Internet  10.8.0.11               4   0008.5d87.d085  ARPA   Vlan8
Internet  10.8.0.12               2   0050.56ab.8fd0  ARPA   Vlan8
Internet  10.8.0.100              9   0008.5d92.e7ac  ARPA   Vlan8


### am l2
sh mac address-table

show mac address-table | include 98be.94


you don't need layer 3 device, will work fine on layer 2 switch as long ip & gateway are configured.

For eg. on Cisco 2960G switch.

c2960#sh mac address-table interface gi0/3
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
  32    68b5.99fc.d1df    DYNAMIC     Gi0/3
Total Mac Addresses for this criterion: 1
c2960#sh ip arp  68b5.99fc.d1df
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.10.32.6              0   68b5.99fc.d1df  ARPA   Vlan32
c2960#
