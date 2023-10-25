
## Outlook Caches  

### vorgeschlagene Kontakte  
grundsätzlich lassen sich "Vorgeschlagene Kontakte" in Outlook einfach beim schreiben, wenn sie eingeblended werden mit ENTF oder dem Kreuzchen löschen  

.n2k Dateien  

### Outlook Cache  

- Outlook schließen (Systray!)
-  %userprofile%\AppData\Local\Microsoft\Windows\INetCache\Content.Outlook
-  kompletten Inhalt leeren
-  %userprofile%\AppData\Local\Microsoft\Outlook\HubAppFileCache
-  ebenso


### Office Cache leeren  
- %LOCALAPPDATA%\Microsoft\Office\16.0\Wef\  (Beispiel 16 )   
- %userprofile%\AppData\Local\Packages\Microsoft.Win32WebViewHost_cw5n1h2txyewy\AC\#!123\INetCache\
- 


## Wenn Admin Rechte benötigt werden  

Oft "braucht" ein Programm Adminrechte zum ausführen. Dies liegt oft daran, dass der angemeldete, limitierte Benutzer keine ausreichenden REchte auf die Ordner hat. Das ist auszuprobieren  

"I've found that most programs that "require" admin rights to run need them because they write to a folder in Program Files or ProgramData, or something like that. We've gotten around some of them by just changing the permissions on those folders to allow "normal" users to modify them."
https://community.spiceworks.com/topic/2209705-program-requires-admin-access-to-run-on-a-standard-account

Shortcut erstellen:  

https://www.howtogeek.com/124087/how-to-create-a-shortcut-that-lets-a-standard-user-run-an-application-as-administrator/  


Ansonsten hat jedes Benutzerkonte Vollzugriff auf den eigenen %appdata% Ordner. Dies macht sich zb. auch MS Teams zuNutzen, damit es jeder Benutzer für sich installieren kann...  

## Windows Firewall Logging  

Firewall Logging ist per Default auf %systemroot%\system32\LogFiles\Firewall gesetzt. Ich setze gern eigene Log Dateien für die 3 Modi (Domain, Privat, Public).  
Jedoch kann es passieren, dass gar keine Log Dateien erzeugt werden... und auch der Ordner "Firewall" gar nicht existiert. Eigentlich sollte er per Default erzeugt werden.  

Im Netz findet man viele Hinweise darauf.  
Es ist nötig, dass der lokale Dienst "NT SERVICE\MpsSvc" Vollzugriff auf den Ordner hat. Ich berechtige hier auf den "LogFiles" Ordner, damit auch der Firewall Unterordner automatisch erzeugt wird.  
Natürlich reicht der Firewall Ordner auch, wenn man eh explizit diesen angibt.  

Sehr ärgerlich...  


# Treiber exportieren  

#Treiber eines laufendes Systems exportieren, wenn man kein Tool (zb. DoubleDriver Portable) verwenden will  

-Terminal als Administrator starten  
-Ausgabeverzeichnis erstellen (zb. c:\install)
- *pnputil /export-driver * c:\install*  


# Windows Update Cache auf Client resetten  

net stop wuauserv  
net stop cryptSvc  
net stop bits  
net stop msiserver  
cd %systemroot%  
rename SoftwareDistribution SoftwareDistribution.old  
rmdir /q /s c:\windows\temp  
net stop trustedinstaller  
c:  
cd c:\windows\logs\CBS  
del *.cab  
del *.log  
rem regenerate cab files  
net start wuauserv  
net start cryptSvc  
net start bits  
net start msiserver  
c:\windows\system32\wuauclt.exe /detectnow  
net start wuauserv  
echo this is a dummy line  


# WSUSOffline  

#einfaches updaten via Script  
DoUpdate /updatecpp /instdotnet35 /updatecerts /verify  
