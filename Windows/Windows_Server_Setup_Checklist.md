# Checkliste beim aufsetzen eines neuen Windows Servers


Normale Windows Server Installation (Desktopoberfläche)  
Windows Updates online installieren  

<ins>Treiberinstallation</ins>  
Treiber-ISO (zb. von HPE)  
Serversoftware (zb. von HPE)  

fixe IP vergeben oder sicherstellen, dass (vorläufig) per DHCP Reservierung eine fixe ausgestellt wird  
WINS / NetBIOS deaktivieren (sofern nicht explizit benötigt)  
RDP aktivieren  
IPv6 deaktivieren ( ?? wirklich ?? )  

wf.msc anheften  
Windows OS LizenzKey eintragen und aktivieren  

Logging aktivieren - 3 Logs pro Netzwerkmodus
Servername ändern

WSUS ID resetten

#AzureArc deaktivieren  
Remove-WindowsFeature AzureArcSetup  
Disable-WindowsOptionalFeature -Online -FeatureName AzureArcSetup  

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


#Windows FW Regeln ausdünnen  
```
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
```

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

..................
weitere:
https://www.linkedin.com/pulse/windows-server-security-checklist-nitesh-remaje  


Install the latest service packs and hotfixes from Microsoft.  

Enable automatic notification of patch availability.  

Set minimum password length.  

Enable password complexity requirements.  

* Do not store passwords using reversible encryption.

* Configure account lockout policy.

* Restrict the ability to access this computer from the network to Administrators and Authenticated Users.

*Do not grant any users the 'act as part of the operating system' right.

* Restrict local logon access to Administrators.

* Deny guest accounts the ability to logon as a service, batch job, locally or via RDP

* Place the warning banner in the Message Text for users attempting to log on.

* Disallow users from creating and logging in with Microsoft accounts.

* Disable the guest account.

* Require Ctrl+Alt+Del for interactive logins.

* Configure machine inactivity limit to protect idle interactive sessions. Compliance

* Configure Microsoft Network Client to always digitally sign communications.

* Configure Microsoft Network Client to digitally sign communications if the server agrees.

* Disable the sending of unencrypted passwords to third-party SMB servers.

* Configure Microsoft Network Server to always digitally sign communications.

* Configure Microsoft Network Server to digitally sign communications if the client agrees.

* Disable anonymous SID/Name translation.

* Do not allow anonymous enumeration of SAM accounts.

* Do not allow anonymous enumeration of SAM accounts and shares.

* Do not allow everyone's permissions to apply to anonymous users.

* Do not allow any named pipes to be accessed anonymously.

* Restrict anonymous access to named pipes and shares.

* Do not allow any shares to be accessed anonymously.

* Require the "Classic" sharing and security model for local accounts.

* Allow Local System to use computer identity for NTLM.

* Disable Local System NULL session fallback.

* Configure allowable encryption types for Kerberos.

* Do not store LAN Manager hash values.

* Set LAN Manager authentication level to only allow NTLMv2 and refuse LM and NTLM.

* Enable the Windows Firewall in all profiles (domain, private, public).

* Configure the Windows Firewall in all profiles to block inbound traffic by default.

* Configure Windows Firewall to restrict remote access services (VNC, RDP, etc.) to authorized organization-only networks.

* Configure Windows Firewall to restrict remote access services (VNC, RDP, etc.) to the organization's VPN.

* Digitally encrypt or sign secure channel data (always).

* Configure machine inactivity limit to protect idle interactive sessions.

* Require strong (Windows 2000 or later) session keys.

* Configure the number of previous logons to cache.

* Configure Account Logon audit policy.

* Configure Account Management audit policy.

* Configure Logon/Logoff audit policy.

* Configure Policy Change audit policy & Privilege Use audit policy.

* Configure Event Log retention method and size.

* Configure log shipping (e.g. to Splunk, Graylog).

* Disable or uninstall unused services.

* Configure user rights to be as secure as possible: Follow the Principle of Least Privilege.

* Ensure all volumes are using the NTFS file system.

* Configure file system as well as registry permissions.

* Disallow remote registry access if not required.

* Set the system date/time and configure it to synchronize against Organization time servers.

* Install and enable anti-spyware and antivirus software.42 Configure anti-virus software to update daily.

* Configure anti-spyware software to update daily.

* Provide secure storage for Confidential (category-I) Data as required. Security can be provided by means such as, but not limited to, encryption, access controls, file system audits, physically securing the storage media, or any combination thereof as deemed appropriate.

* Install software to check the integrity of the critical operating system files.

* If RDP is utilized, set the RDP connection encryption level to high.



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

.................
weiters Dienste deaktivieren:  
https://www.der-windows-papst.de/2019/12/01/windows-server-unnoetige-dienste-abschalten/  

@echo off 
 
sc qc AeLookupSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config AeLookupSvc start= disabled  > nul
sc stop AeLookupSvc  > nul
)

sc qc Browser | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config Browser start= disabled  > nul
sc stop Browser  > nul
)

sc qc WPDBusEnum | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config WPDBusEnum start= disabled  > nul
sc stop WPDBusEnum  > nul
)

sc qc fdPHost | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config fdPHost start= disabled  > nul
sc stop fdPHost  > nul
)

sc qc SharedAccess | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config SharedAccess start= disabled  > nul
sc stop SharedAccess  > nul
)

sc qc IPBusEnum | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config IPBusEnum start= disabled  > nul
sc stop IPBusEnum  > nul
)

sc qc RemoteAccess | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config RemoteAccess start= disabled  > nul
sc stop RemoteAccess  > nul
)

sc qc UxSms | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config UxSms start= disabled  > nul
sc stop UxSms  > nul
)

sc qc ShellHWDetection | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config ShellHWDetection start= disabled  > nul
sc stop ShellHWDetection  > nul
)

sc qc SSDPSRV | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config SSDPSRV start= disabled  > nul
sc stop SSDPSRV  > nul
)


sc qc upnphost | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config upnphost start= disabled  > nul
sc stop upnphost  > nul
)

sc qc TrkWks | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config TrkWks start= disabled  > nul
sc stop TrkWks  > nul
)

sc qc CertPropSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config CertPropSvc start= disabled  > nul
sc stop CertPropSvc  > nul
)

sc qc MapsBroker | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config MapsBroker start= disabled  > nul
sc stop MapsBroker > nul
)

sc qc lfsvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config lfsvc start= disabled  > nul
sc stop lfsvc > nul
)

sc qc AxInstSV | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config AxInstSV start= DEMAND > nul
sc stop AxInstSV > nul
)

sc qc bthserv | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config bthserv start= disabled  > nul
sc stop bthserv  > nul
)

sc qc dmwappushservice | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config dmwappushservice start= disabled  > nul
sc stop dmwappushservice  > nul
)

sc qc lltdsvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config lltdsvc start= DEMAND > nul
sc stop lltdsvc > nul
)

sc qc NgcSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
sc config NgcSvc start= DEMAND > nul
sc stop NgcSvc > nul
)

sc qc NgcCtnrSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config NgcCtnrSvc start= DEMAND > nul
sc stop NgcCtnrSvc > nul
)

sc qc NcbService | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config NcbService start= disabled  > nul
sc stop NcbService  > nul
)

sc qc PhoneSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config PhoneSvc start= DEMAND > nul
sc stop PhoneSvc > nul
)

sc qc RmSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config RmSvc start= disabled  > nul
sc stop RmSvc  > nul
)

sc qc SensorDataService | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config SensorDataService start= DEMAND > nul
sc stop SensorDataService > nul
)


sc qc SensrSvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config SensrSvc start= DEMAND > nul
sc stop SensrSvc > nul
)

sc qc ScDeviceEnum | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config ScDeviceEnum start= DEMAND > nul
sc stop ScDeviceEnum > nul
)


sc qc WiaRpc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config WiaRpc start= DEMAND > nul
sc stop WiaRpc > nul
)

sc qc upnphost | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config upnphost start= disabled  > nul
sc stop upnphost  > nul
)

sc qc WalletService | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config WalletService start= DEMAND > nul
sc stop WalletService > nul
)

sc qc Audiosrv | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config Audiosrv start= disabled  > nul
sc stop Audiosrv  > nul
)

sc qc AudioEndpointBuilder | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config AudioEndpointBuilder start= disabled  > nul
sc stop AudioEndpointBuilder  > nul
)

sc qc FrameServer | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config FrameServer start= disabled  > nul
sc stop FrameServer  > nul
)

sc qc icssvc | %SYSTEMROOT%\system32\find.exe /i /c "disabled" > nul
if not %errorlevel%==0 (
sc config icssvc start= disabled  > nul
sc stop icssvc  > nul
)


