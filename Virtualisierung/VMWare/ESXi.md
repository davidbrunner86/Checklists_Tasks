
## Befehle

#Depot File (Zip) herunterladen und auf lokalen Storage ablegen  
#Pfad dann ggf. wie folgt:  
/vmfs/volumes/605b037a-833b04a4-7d02-2c768a5d8a18/ISO/VMware-ESXi-7.0.1-17325551-HPE-701.0.0.10.6.3.9-Jan2021-depot.zip  

#Profile darin auflisten  
esxcli software sources profile list -d /vmfs/volumes/605b037a-833b04a4-7d02-2c768a5d8a18/ISO/VMware-ESXi-7.0.1-17325551-HPE-701.0.0.10.6.3.9-Jan2021-depot.zip  

Ergebnis ggf:  
HPE-Custom-AddOn_701.0.0.10.6.3-9  

#auf dieses updaten  
esxcli software profile update -d /vmfs/volumes/605b037a-833b04a4-7d02-2c768a5d8a18/ISO/VMware-ESXi-7.0.1-17325551-HPE-701.0.0.10.6.3.9-Jan2021-depot.zip -p HPE-Custom-AddOn_701.0.0.10.6.3-9


#Online-Variante des auflisten:  
esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml  | grep -i ESXi-7  

#Auf eines davon updaten:  
esxcli software profile update -p ESXi-7.0U1d-17551050-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml

#Wenn obiger Befehl eine Hardware-Inkompatibilität (zb. CPU Support) meldet, kann man dieses umgehen:  
esxcli software profile update -p ESXi-7.0U1d-17551050-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml --no-hardware-warning  




## ESXI Update August 23

In Reihenfolge:  
VMs verschieben (VLSC VM kann bleiben)  
Host in Wartungsmodus versetzen  
In Vcenter: Host - Configure - Services - SSH starten  
Mit Administratorlogin per SSH auf Host verbinden  
Kommando:  
esxcli software profile update -p ESXi-7.0U3n-21930508-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml  
Reboot  
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


.....................

## ESXi Tools updaten per Baseline  
https://thesleepyadmins.com/2021/07/01/updating-vmware-tools-on-esxi-7-0-host-using-vmware-lifecycle-manager/  


................
## VLAN SVR Core change

https://kb.vmware.com/s/article/2084629

Mgmt IP change direkt an der konsole. Einstellung vlan id nicht vergessen. zur sicherheit neustart ESXI, wenn eh Wartungsmodus, was empfehlenswert ist

ssh in vcenter appliance  
kein Shell nötig  

neustart des DNS Resolvers:  
com.vmware.appliance.version1.services.restart --name dnsmasq
