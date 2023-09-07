
## ESXI Update August 23

esxcli software profile update -p ESXi-7.0U3n-21930508-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml

................
## ESXi Tools Update

#Vmware Version  
vmware -v  
#Beispiel: VMware ESXi 7.0.3 build-21930508

#Tools Version  
esxcli software vib list | grep tools  
#Beispiel tools-light                    12.1.5.20735119-21422485               VMware  VMwareCertified   2023-08-18  

#Verfügbare Tools anzeigen  
esxcli software sources vib list --depot=https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml | grep tools-light | sort  

#nächste Version zum beispiel: 12.2.5.21855600-22082334  

#Update auf neueste Version   
esxcli software vib update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml --vibname tools-light

#maintenance mode  
#reboot  

#Version wieder anzeigen  
vmware -v  



................
## VLAN SVR Core change

https://kb.vmware.com/s/article/2084629

Mgmt IP change direkt an der konsole. Einstellung vlan id nicht vergessen. zur sicherheit neustart ESXI, wenn eh Wartungsmodus, was empfehlenswert ist

ssh in vcenter appliance  
kein Shell nötig  

neustart des DNS Resolvers:  
com.vmware.appliance.version1.services.restart --name dnsmasq
