# Befehle in NetSH  

## Netsh für DHCP  

netsh  
dhcp server  

### Füge eine Reservierung für PC XY hinzu:  
scope 10.1.1.0 add reservedip 10.1.1.5 ab1a2e123456 samplehost.sample.local "PC SampleUser"


# Remoteverwaltung

### PsExec

Remoteverwaltung Netsh mit PsExec, Berechtigungen (DomainAdmin, ..) vorrausgesetzt

c:\psexec \\remote_machine_name cmd  

#Disable Firewall
netsh advfirewall set currentprofile state off

#Alternativ RDP zulassen und FW aktiv lassen:
netsh advfirewall firewall set rule group=”remote desktop” new enable=Yes
reg add “HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server” /v fDenyTSConnections /t REG_DWORD /d 0 /f
Go back to the Services MMC you used in Step 2 Option 2, find the service “Remote Desktop Services” and start it (or restart if it is already running).

weiteres in der CMD...
services.mmc  



## Statische und Dynamische IP gleichzeitig auf einer NIC  

Seit Windows 10 1703 kann man parallel eine statische und eine dynamische IP (also DHCP aktiv) auf einer NIC betreiben.  

    REM Enable dhcpstaticipcoexistence to allow for both DHCP and Static IP  
    netsh interface ip set interface interface="Ethernet" dhcpstaticipcoexistence=enable  

    REM Configure the interface to use DHCP (if needed)  
    netsh interface ip set address "Ethernet" dhcp  

    REM Add secondary Static IP (takes a few seconds to show up in ipconfig)  
    netsh int ip add address "Ethernet" 192.168.0.1 255.255.255.0  

    REM Delete the IP (if needed)  
    netsh int ip delete address "Ethernet" 192.168.0.1 255.255.255.0  

Quelle: https://trae.sk/view/34/  
