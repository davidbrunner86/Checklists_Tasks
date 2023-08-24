
## ESXI Update August 23

esxcli software profile update -p ESXi-7.0U3n-21930508-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml

................
## VLAN SVR Core change

https://kb.vmware.com/s/article/2084629

Mgmt IP change direkt an der konsole. Einstellung vlan id nicht vergessen. zur sicherheit neustart ESXI, wenn eh Wartungsmodus, was empfehlenswert ist

ssh in vcenter appliance  
kein Shell nötig  

neustart des DNS Resolvers:  
com.vmware.appliance.version1.services.restart --name dnsmasq
