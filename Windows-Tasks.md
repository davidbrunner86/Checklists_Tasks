
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
