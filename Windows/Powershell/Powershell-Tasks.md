## Allgemeine Linkliste für Anfänger:  

https://administrator.de/knowledge/powershell-leitfaden-fuer-anfaenger-768927593.html  

https://itelv.de/index.php/software-projekte/powershell


## Einfach schnell eine fixe IP setzen  

Erst die ID des Netzwerkadapters bestimmen:  

Get-NetAdapter  

hier dann den "ifIndex" notieren für den Adapter, auf dem man die IP setzen will  

Dann das Script:  

New-NetIPAddress –IPAddress 192.168.1.13 -DefaultGateway 192.168.1.1 -PrefixLength 24 -InterfaceIndex IfIndex  


## Eventlog  
Get-WindowsUpdateLog  

--> export .etl Logs win10/server2016 auf Desktop  


## GMSA Account erstellen  

New-ADServiceAccount -Name gMSA_SERVICE -DNSHostName gMSA_SERVER.sample.local -PrincipalsAllowedToRetrieveManagedPassword SERVERNAME$


## Einfaches Log Script Infinity  

<#  
Log Kommentar  
#>  

#Variablen  
$FilePath = "C:\Temp\work\IN"  
$LogPathName = "c:\Scripts\LogContent.log"  
    
#Log-Funktion  
Function WriteLog {  
    Param ([string]$LogString)  
    $Stamp = Get-Date  
    $LogMessage = "$Stamp - $LogString"  
    Add-Content $LogPathName -value $LogMessage  
    }  

#los gehts  

$FileList = Get-ChildItem –Path $FilePath  

get-date >> "c:\temp\TestLog.log"  
"*****************" >> "c:\temp\TestLog.log"  

#loop infinite  
while (1 -eq 1 ){  

foreach($item in $FileList) {  
    WriteLog $item  
}  

start-sleep -seconds 5  
}  


## NETSH Wert per Loop abfragen  

#Server aus dem AD holen und pro Server den TCPAutoTuning Wert per Netsh abfragen  

Import-Module ActiveDirectory  
$servers = Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' -Properties Name,Operatingsystem,OperatingSystemVersion,IPv4Address | Sort-Object -Property Operatingsystem | Select-Object -Property Name  

foreach ($server in $servers){  
Invoke-Command -ScriptBlock { Get-NetTCPSetting -Setting Internet | select AutoTuningLevelLocal } -ComputerName $server.Name  
}  


# Dateitypen eines Unterordners auflisten  

Get-ChildItem "c:\temp" -Recurse -file | select Extension -Unique  
# Hotfix remote abfragen  

Get-HotFix -id KB4577586 -ComputerName Server01.domain.local, server02 -Credential $Credentials


# Wenn Wochenende, breche ab  
if (( @(6,0).foreach(  {$_ -eq [int](Get-Date).dayofweek} ) -contains $true )) {exit 2}  
