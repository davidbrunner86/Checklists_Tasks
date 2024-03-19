## Script to remote invoke WU Scan and Update  

Import-Module ActiveDirectory  
$servers = Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' -Properties Name,Operatingsystem,OperatingSystemVersion | Sort-Object -Property Operatingsystem | Select-Object -Property Name  

foreach ($server in $servers){  

$Servername = $server.Name  
Write-Host "Executing $servername"  

if($Servername -like "Testserver1"){continue}  
if($Servername -like "anothertest"){continue}  

Invoke-Command -ComputerName $Servername -ScriptBlock{  

$updateSession = new-object -com "Microsoft.Update.Session"; $updates=$updateSession.CreateupdateSearcher().Search($criteria).Updates  
wuauclt /reportnow  

#"C:\Program Files\Windows Defender\MpCmdRun.exe" -removedefinitions -dynamicsignatures  maybe another thing?  

cleanmgr /autoclean  

#Notworking...  
#"%ProgramFiles%\Windows Defender\MpCmdRun.exe -removedefinitions -dynamicsignatures"  
#MpCmdRun.exe -SignatureUpdate  

}  

}  

