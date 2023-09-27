## Allgemeine Linkliste für Anfänger:  

https://administrator.de/knowledge/powershell-leitfaden-fuer-anfaenger-768927593.html  

https://itelv.de/index.php/software-projekte/powershell


## Einfach schnell eine fixe IP setzen  

Erst die ID des Netzwerkadapters bestimmen:  

Get-NetAdapter  

hier dann den "ifIndex" notieren für den Adapter, auf dem man die IP setzen will  

Dann das Script:  

New-NetIPAddress –IPAddress 192.168.1.13 -DefaultGateway 192.168.1.1 -PrefixLength 24 -InterfaceIndex IfIndex  

