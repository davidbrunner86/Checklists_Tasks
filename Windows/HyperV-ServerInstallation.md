# Installation eines Hyper-V Servers ( 2019 ) von ISO  

Unformatierte HDD als Ganzes als Ziel verwenden  
Administratorkennwort vergeben  

manuell starten:  
C:\windows\system32\de-DE\sconfig.vbs  
(ist auch nach jedem Neustart nötig)  

(2) Computername ändern  
neustart  

(7) remotedesktop aktivieren - Option 1 (höhere Sicherheit)  
(1) zur Domäne hinzufügen  

(4) remoteverwaltung konfigurieren - 1 aktivieren, 3 ping aktivieren  

wsus mitgliedschaft pflegen  
(6) windows updates installieren  

weitere Installationen via RDP möglich  

Firewallregeln pflegen:  

netsh advfirewall firewall set rule group="Remotedesktop" new enable=yes  
netsh advfirewall firewall set rule group="Windows-Remoteverwaltung" new enable=yes  
netsh advfirewall firewall set rule group="Datei- und Druckerfreigabe" new enable=yes  
netsh advfirewall firewall set rule group="Remotedienstverwaltung" new enable=yes  
netsh advfirewall firewall set rule group="Leistungsprotokolle und -warnungen" new enable=yes  
Netsh advfirewall firewall set rule group="Remote-Ereignisprotokollverwaltung" new enable=yes  
Netsh advfirewall firewall set rule group="Remoteverwaltung geplanter Aufgaben" new enable=yes  
netsh advfirewall firewall set rule group="Remotevolumeverwaltung" new enable=yes  
netsh advfirewall firewall set rule group="Windows-Firewallremoteverwaltung" new enable=yes  
netsh advfirewall firewall set rule group="Windows-Verwaltungsinstrumentation (WMI)" new enable=yes  


sconfig.vbs kopieren von c:\windows\system32\de-DE nach en-US  

Im Hyper-v Manager:  
- virtuellen Switch (extern) erstellen für LAN Zugriff


IPv6 deaktivieren:  
To completely disable IPv6 on a Windows Server 2008-based computer yourself, follow these steps:  

Open Registry Editor.  
Locate the following registry subkey:  
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesTcpip6Parameters  
In the details pane, click New, and then click DWORD (32-bit) Value.  
Type DisabledComponents, and then press ENTER.  
Double-click DisabledComponents, and then type 0xffffffff in Hexadecimal or 4294967295 in Decimal.Note The 0xffffffff value or the 4294967295 value disables all IPv6 components except for the IPv6 loopback interface.  


https://administrator.de/knowledge/feature-arbeiten-windows-2019-coreservern-erleichtern-517555.html  
https://docs.microsoft.com/en-us/windows-server/get-started-19/install-fod-19  


Im Problemfall kann man sich immer mit localhost\administrator remote anmelden  
