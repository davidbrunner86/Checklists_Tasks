# Ein ganz einfaches, Basis Ping-Script für einfaches Netzwerk Monitoring (besser als nix! und ist schnell gepflegt und im Fehlerfall kann es helfen)  

#contoso.com durch eigene (lokale) domain ersetzen

### manueller One-Liner  

#Einfacher PC  
if (-not (Test-Connection 10.1.30.46 -quiet -count 10)) {send-mailmessage -to "admin@contoso.com" -from "backup@contoso.com" -smtpServer "mail.contoso.com" -subject "PCA - 10.1.30.46 - nicht erreichbar"}

#Internetzugriff
if (-not (Test-Connection 8.8.8.8 -quiet -count 10)) {send-mailmessage -to "admin@contoso.com" -from "backup@contoso.com" -smtpServer "mail.contoso.com" -subject "Kein Internetzugriff?"}  

### Server aus dem AD holen und per schleife prüfen:  

#AD-Import
Import-Module ActiveDirectory  
$servers = Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' -Properties Name,Operatingsystem,OperatingSystemVersion,IPv4Address | Sort-Object -Property Operatingsystem | Select-Object -Property Name  

#$datetime=[datetime]::Today  
#$DateTimeNow = Get-Date  

foreach ($server in $servers){  
$Servername = $server.Name  

#EOL bzw. prinzipiell ausgeschaltet  - nachfolgend sind Ausnahmen definiert  
#if($Servername -like "EOL-Server-Example"){continue}  

#Jetzt testet er
if (-not (Test-Connection $server.Name -quiet -count 10)) {send-mailmessage -to "admin@contoso.com" -from "backup@contoso.com" -smtpServer "mail.contoso.com" -subject "$Servername - nicht erreichbar"}
}

### Einzelne Checks nur zu bestimmter Zeit  

#Zwischen 5 und 10, also Anfangszeit BusinessHours  
if ($DateTimeNow.TimeOfDay.Hours -ge 5 -and $DateTimeNow.TimeOfDay.Hours -le 10){  

# ...
}  


### Port Tests  

#Ticketsystem auf Port 443  
if (-not (Test-NetConnection -port 443 -ComputerName ticket.contoso.com)) {send-mailmessage -to "admin@contoso.com" -from "backup@contoso.com" -smtpServer "mail.contoso.com" -subject "Ticketsystem Port 443 nicht erreichbar"}  

### Services Test

#Exchange Transportdienst  
if ((Get-Service -ComputerName InternerMailServer -Name "MSExchangeTransport").status -ne "Running") {send-mailmessage -to "admin@contoso.com" -from "backup@contoso.com" -smtpServer "mail.contoso.com" -subject "Exchange Dienst MSExchangeTransport läuft nicht"}  

