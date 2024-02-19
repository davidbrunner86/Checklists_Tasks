## IIS Logs für FTP ansehen  
Import-Module -Name WebAdministration  
$LogfileDirectory = (Get-ItemProperty -Path ‘IIS:\Sites\ftp.domain.com’ -Name logfile).directory  

$LogfileFolder = [System.Environment]::ExpandEnvironmentVariables(“$LogfileDirectory”)  

Function Out-GridViewIISLog ($File) {  
   #.Synopsis  
   #Convert IIS log file to CSV and display in a GridView  
   #.LINK  
   #Based on https://stevenaskwith.com/2012/05/22/parse-iis-log-files-with-powershell/  
   #Performance inspired by http://www.happysysadm.com/2014/10/reading-large-text-files-with-powershell.html  
   ###########################################################################################################  
   $Headers = @((Get-Content -Path $File -ReadCount 4 -TotalCount 4)[3].split(' ') | Where-Object { $_ -ne '#Fields:' });  
   Import-Csv -Delimiter ' ' -Header $Headers -Path $File | Where-Object { ($_.date -notlike '#*') -and ($_.'x-fullpath' -like "*.xml*") } | Select-Object -Property date, time, c-ip, cs-username, x-fullpath | Out-GridView -Title "IIS log: $File - mit .xml filtern";  

};  

#vortag  
Out-GridViewIISLog -File "E:\inetpub\logs\LogFiles\FTPSVC2\u_ex$(get-date((Get-Date).AddDays(-1)) -F 'yyMMdd').log"  

#aktuell  

Out-GridViewIISLog -File "E:\inetpub\logs\LogFiles\FTPSVC2\u_ex$(Get-Date -F 'yyMMdd').log"  

#temporär fixiert:  

Out-GridViewIISLog -File "E:\inetpub\logs\LogFiles\FTPSVC2\u_ex230906.log"  

# Zeig mir einfach die Liste der Logs  

Get-ChildItem -Path $LogfileFolder -Recurse | Out-GridView -Wait  

<#  
Es öffnen sich (aktuell) 3 Fenster  
Ein Fenster mit den Files des aktuellen Tages  
ein Fenster mit Beispielen vom 6.9.23 (aktuell zum Vergleich)  
ein Fenster mit der Logliste  

damit kann man vergleichen und sieht sofort, was am aktuellen Tag so bisher kam, was zum vergleich am 6.9.23 war und auch, dass Logs geschrieben wurden  

#>  
