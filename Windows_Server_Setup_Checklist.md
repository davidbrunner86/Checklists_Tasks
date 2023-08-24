# Checkliste beim aufsetzen eines neuen Windows Servers


Normale Windows Server Installation (Desktopoberfläche)  
Windows Updates online installieren  

Treiberinstallation  
HPE Treiber-ISO  
HPE serversoftware  


wf.msc anheften  
Key eintragen und aktivieren  

Logging aktivieren - 3 Logs pro Netzwerkmodus
Servername ändern

WSUS ID resetten

.....

RDP aktiviert (temporär)  
Teamviewer Host installiert (temporär ?)  
3rd Party AntiVirus Programm installiert (nachdem Servername korrekt)  
Vollscan durch AV Programm ggf.  
Einstellung: Konten- Apps wieder starten   


7zip  

VEEAM  
veeam B&R installation  
B&R Webconsole  
VeeamOne Ports:  
2714,2742,2741,1239,2805  

Dienst "Remoteregistrierung" deaktivieren  

Windows SErver Funktion "Multipath I/O" installieren, damit FC Verbindungen nicht doppelt angezeigt werden im Diskmanager  
 

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
