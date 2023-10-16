### Einfach nur ein paar simple Scripts, die sofort zu nutzen sind und hilfreich sind  

## Installationen nacheinander mit Wartezeit dazwischen:  
\\softwaredepot\install1.exe /auto
timeout 60 /nobreak
start /wait \\softwaredepot\Install-Update.exe /auto
timeout 30 /nobreak
xcopy \\vorlage\shortcut.lnk C:\Users\Public\Desktop


## WMI Scripts  

### Standarddrucker abfragen  

Get-WmiObject -query "SELECT * FROM WIN32_PRINTER WHERE Default = TRUE"  

#Steht hier:
#HKEY_USERS\S-1-5-21-329068152-507921405-1060284298-10572\Software\Microsoft\Windows NT\CurrentVersion\Windows

#Standarddrucker anzeigen:
wmic printer where "Default = 'TRUE'" get Caption  



## Diverse 

ich habe folgenden Code um in einem Hauptordner mittels einer Schleife alle Unterordner zu suchen und jeden gefunden Ordner mit der Konsole von PowerArchiver zu zippen:  
...  
Hierbei werden die Zip-Dateien im Startpfad abgelegt (= %%d.zip), wegen der Übersicht wegen möchte ich aber folgenden Speicherpfad verwenden: D:\Sicherung\Box\zip\  

In %%d ist der komplette Pfad mit der dynamischen Dateibezeichnung enthalten.  

Wie muss man die bestehende Zeile ändern um den Pfad, welcher in %%d enthalten ist, durch einen anderen Pfad zu ersetzen damit die *.zip-Dateien woanders gespeichert werden?  

Lösung:  
for /D %%d in ("C:\temp\*") do "C:\PACL\pacomp64.exe" -a -c0 -r "D:\Sicherung\Box\zip\%%~nxd.zip" "%%d"  





### FTP Script  

zb. FTP-conn.cmd  

open ftp.server.com  
domain\FTP_Datenaustausch  
S1cheresP@sswort  

ls  
dir  

lcd C:\temp  
put samplefile.txt  

quit  


## Office365  

### manuelles Update  
"c:\Program Files\Common Files\microsoft shared\ClickToRun\OfficeC2RClient.exe" /update user  


## MAC lokal auflisten
getmac  


## Kurzer Speedcheck lokaler Storage  
winsat.exe disk -drive c:  


## DomainController auflisten von beliebiger Workstation/Server  
netdom query /d:domain.local DC  

