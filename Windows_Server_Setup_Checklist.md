# Checkliste beim aufsetzen eines neuen Windows Servers


Normale Windows Server Installation (Desktopoberfläche)  
Windows Updates online installieren  

<ins>Treiberinstallation</ins>  
Treiber-ISO (zb. von HPE)  
Serversoftware (zb. von HPE)  


wf.msc anheften  
Windows OS LizenzKey eintragen und aktivieren  

Logging aktivieren - 3 Logs pro Netzwerkmodus
Servername ändern

WSUS ID resetten

.....

RDP aktiviert (temporär)  
Teamviewer Host installiert (temporär ?)  
3rd Party AntiVirus Programm installiert (nachdem Servername korrekt)  
Vollscan durch AV Programm ggf.  
Einstellung: Konten- Apps wieder starten   
Dienst "Remoteregistrierung" deaktivieren  

Standardsoftware installieren  
7zip  

<ins>Vorlage Veeam</ins>  
VEEAM  
veeam B&R installation  
B&R Webconsole  
VeeamOne Ports:  
2714,2742,2741,1239,2805  


Bei FC Block Storage:  
Windows SErver Funktion "Multipath I/O" installieren, damit FC Verbindungen nicht doppelt angezeigt werden im Diskmanager  
--- hier fehlt Bild --- 

schadet zwischendurch nicht:  
ipconfig /flushdns && ipconfig /registerdns  


.............................................................

ausständig:
Veeam One einrichten, restriktive Userberechtigungen
gMSA Accounts, Add-WindowsFeature RSAT-AD-PowerShell

firewallregeln strikt definieren  !! anpassen! Teils musste man sie deaktivieren
NIC Teaming konfigurieren
IPs fix setzen
SAN HBAs konfigurieren (?)

Vorlage:
WSUS ID Reset:
net stop wuauserv 
reg Delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate /v PingID /f 
reg Delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate /v AccountDomainSid /f 
reg Delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate /v SusClientId /f  
reg Delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate /v SusClientIDValidation /f 
net start wuauserv 
wuauclt.exe /resetauthorization /detectnow

winrm quickconfig
Enable-PSRemoting
firewall regeln: Icmp, RDP (?), UDP 389 (AD, ...)
windows defender ?
bei hyper-v : floppy entfernen:
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\flpydisk -- start von 3 auf 4 ändern


Windows FW Regeln ausdünnen
 $FirewallRules = @(
„AllJoyn-Router (TCP eingehend)“,
„AllJoyn-Router (TCP ausgehend)“,
„AllJoyn-Router (UDP eingehend)“,
„AllJoyn-Router (UDP ausgehend)“,
„Erfassungsportalablauf“,
‚“Wiedergabe auf Gerät“-Funktionalität (qWave-TCP eingehend)‘,
‚“Wiedergabe auf Gerät“-Funktionalität (qWave-TCP ausgehend)‘,
‚“Wiedergabe auf Gerät“-Funktionalität (qWave-UDP eingehend)‘,
‚“Wiedergabe auf Gerät“-Funktionalität (qWave-UDP ausgehend)‘,
‚“Wiedergabe auf Gerät“-SSDP-Suche (UDP eingehend)‘,
‚“Wiedergabe auf Gerät“-Streamingserver (HTTP-Streaming eingehend)‘,
‚“Wiedergabe auf Gerät“-Streamingserver (RTCP-Streaming eingehend)‘,
‚“Wiedergabe auf Gerät“-Streamingserver (RTP-Streaming ausgehend)‘,
‚“Wiedergabe auf Gerät“-Streamingserver (RTSP-Streaming eingehend)‘,
‚“Wiedergabe auf Gerät“-UPnP-Ereignisse (TCP eingehend)‘,
„Benutzererfahrungen und Telemetrie im verbundenen Modus“,
„Desktop-App-Web-Viewer“,
„DIAL-Protokollserver (HTTP-In)“,
„E-Mail und Konten“,
„mDNS (UDP eingehend)“,
„mDNS (UDP ausgehend)“,
„Microsoft Media Foundation-Netzwerkquelle IN [TCP 554]“,
„Microsoft Media Foundation-Netzwerkquelle IN [UDP 5004-5009]“,
„Microsoft Media Foundation-Netzwerkquelle OUT [TCP ALL]“,
„Sprachausgabe“,
„Starten“,
„Windows-Standardsperrbildschirm“,
„Windows Feature Experience Pack“,
„Geschäfts- oder Schulkonto“,
„Ihr Konto“
)
foreach ($FirewallRule in $FirewallRules) {
Get-NetFirewallRule -Displayname $FirewallRule | Set-NetFirewallRule -Enabled:false
} 

IIS Logs - Ordner komprimieren

.......................
Best Practices Firewall:
https://activedirectorypro.com/windows-firewall-best-practices/

netsh advfirewall show all  

.........................

## NetConnection Profil (Domäne, Privat, Öffentlich) ändern

Manchmal wird dies falsch erkannt oder ändert sich und muss anders gesetzt werden

Am schnellsten wohl per Regedit:

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles

Powershell:

Get-ChildItem -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\" 
➜ $UUID = "{01234ABC-...}" ➜ $VALUE = "Network1" ➜ (Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\$UUID").ProfileName ➜ 
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\$UUID" -Name ProfileName -Value $VALUE



.....................
weitere Vorlagen:
https://www.alitajran.com/windows-server-post-installation/

Do not start Server Manager automatically at logon  
Turn off Internet Explorer Enhanced Security Configuration  

<img src="https://www.alitajran.com/wp-content/uploads/2021/11/Windows-Server-post-installation-configuration-03.png">


Set time zone  
Check Windows Server network card  
Configure Windows Firewall to allow pings  
Update Windows Server  



......................
weitere Quellen:

https://github.com/MicrosoftDocs/windows-itpro-docs/tree/public/windows/security

https://github.com/MicrosoftDocs/windowsserverdocs/blob/main/WindowsServerDocs/identity/ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md

https://github.com/decalage2/awesome-security-hardening

https://github.com/PaulSec/awesome-windows-domain-hardening

https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj717277(v=ws.11)


...............  
Dienste deaktivieren  
https://www.tech-faq.net/unwichtige-dienste-in-server-2019-deaktivieren/  

Powershell  
Set-Service "XblAuthManager" -StartupType disabled #XBOX Live Authentifizierung  
Set-Service "XblGameSave" -StartupType disabled #XBOX Live-Spiele speichern  

Set-Service "MapsBroker" -StartupType disabled #Manager für heruntergeladene Karten  
Set-Service "lfsvc" -StartupType disabled #Geolocation-Dienst  
Set-Service "AxInstSV" -StartupType disabled #ActiveX-Installer (AxInstSV)  
Set-Service "bthserv" -StartupType disabled #Bluetooth-Unterstützungsdienst  
Set-Service "CDPUserSvc" -StartupType disabled #CDPUserSvc  
Set-Service "PimIndexMaintenanceSvc" -StartupType disabled #Kontaktdaten  
Set-Service "dmwappushservice" -StartupType disabled #dmwappushsvc (WAP Push-Nachrichtenroutingdienst)  
Set-Service "SharedAccess" -StartupType disabled #Gemeinsame Nutzung der Internetverbindung  
Set-Service "lltdsvc" -StartupType disabled #Verbindungsschicht-Topologieerkennungs-Zuordnungsprogramm  
Set-Service "wlidsvc" -StartupType disabled #Anmelde-Assistent für Microsoft-Konten /ACHTUNG: appx-Pakete nicht mehr ausführbar  
Set-Service "NgcSvc" -StartupType disabled #Microsoft Passport  
Set-Service "NgcCtnrSvc" -StartupType disabled #Microsoft Passport-Container  
Set-Service "NcbService" -StartupType disabled #Netzwerkverbindungsbroker  
Set-Service "PhoneSvc" -StartupType disabled #Telefondienst  
Set-Service "PcaSvc" -StartupType disabled #Programmkompatibilitäts-Assistent-Dienst  
Set-Service "QWAVE" -StartupType disabled #Verbessertes Windows-Audio/Video-Streaming  
Set-Service "RmSvc" -StartupType disabled #Funkverwaltungsdienst  
Set-Service "SensorDataService" -StartupType disabled #Sensordatendienst  
Set-Service "SensrSvc" -StartupType disabled #Sensorüberwachungsdienst  
Set-Service "SensorService" -StartupType disabled #Sensordienst  
Set-Service "ShellHWDetection" -StartupType disabled #Shellhardwareerkennung  
Set-Service "ScDeviceEnum" -StartupType disabled #Smartcard-Geräteaufzählungsdienst  
Set-Service "SSDPSRV" -StartupType disabled #SSDP-Suche  
Set-Service "WiaRpc" -StartupType disabled #Ereignisse zum Abrufen von Standbildern  
Set-Service "OneSyncSvc" -StartupType disabled #Synchronisierungshost  
Set-Service "TabletInputService" -StartupType disabled #Dienst für Bildschirmtastatur und Schreibbereich  
Set-Service "upnphost" -StartupType disabled #UPnP-Gerätehost  
Set-Service "UserDataSvc" -StartupType disabled #Benutzerdatenzugriff  
Set-Service "UnistoreSvc" -StartupType disabled #Benutzerdatenspeicher  
Set-Service "WalletService" -StartupType disabled #WalletService  
Set-Service "Audiosrv" -StartupType disabled #Windows-Audio  
Set-Service "AudioEndpointBuilder" -StartupType disabled #Windows-Audio-Endpunkterstellung  
Set-Service "FrameServer" -StartupType disabled #Windows-Kamera-FrameServer  
Set-Service "stisvc" -StartupType disabled #Windows-Bilderfassung (WIA)  
Set-Service "wisvc" -StartupType disabled #Windows-Insider-Dienst  
Set-Service "icssvc" -StartupType disabled #Windows-Dienst für mobile Hotspots  
Set-Service "WpnService" -StartupType disabled #Windows-Pushbenachrichtigungssystemdienst  
Set-Service "WpnUserService" -StartupType disabled #Windows-Pushbenachrichtigungs-Benutzerdienst  


# Wenn kein Printserver  
Set-Service "PrintNotify" -StartupType disabled #Druckererweiterungen und -benachrichtigungen  
# Wenn kein Printserver oder Domaämnencontroller  
Set-Service "Spooler" -StartupType disabled #Druckerwarteschlange  


weiters:  
https://learn.microsoft.com/de-de/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server  

