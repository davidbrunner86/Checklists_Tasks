# einfaches port-forwarding  
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 9090 -j DNAT --to 10.0.0.200:9090  

# Port-forwarding Range  
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 64000:65000 -j DNAT --to 10.0.0.200:61000-62000  
