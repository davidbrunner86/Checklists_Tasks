
$Ports = Get-NetTCPConnection -State Established | select localport -Unique | select localport  

$Ports = $Ports | Where-Object {[int]$_.localport -lt 49151}  



# Ports pro Server:  

Import-Module ActiveDirectory  
$servers = Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' -Properties Name | Select-Object -Property Name  

$datetime=[datetime]::Today  

#$Ports | Where-Object {[int]$_.localport -lt 49151}  

foreach ($server in $servers){  

$Servername = $server.Name  
$Path = "C:\temp\localPortUsage-" + $Servername + ".csv"  

$Ports = Invoke-Command -ComputerName $server.Name {Get-NetTCPConnection -State Established | select localport -Unique } | select localport  

$Ports | Export-Csv -Path $Path -Append  
#Import-Csv C:\work\localPortUsage.csv | Sort localport -Unique | Set-Content C:\work\localPortUsage-$Servername.csv  

$CSV = Import-Csv (Get-ChildItem $Path) | Sort-Object -Unique localport  
rm  $Path  
$CSV = $CSV | Where-Object {[int]$_.localport -lt 49151}  
$CSV | Export-Csv $Path -NoClobber -NoTypeInformation  

}  

$servername = "testserver"  

$Path = "C:\temp\localPortUsage-" + $Servername + ".csv"  

$Ports = Invoke-Command -ComputerName $server.Name {Get-NetTCPConnection -State Established | select localport -Unique } | select localport  

$Ports | Export-Csv -Path $Path -Append  
#Import-Csv C:\work\localPortUsage.csv | Sort localport -Unique | Set-Content C:\work\localPortUsage-$Servername.csv  

$CSV = Import-Csv (Get-ChildItem $Path) | Sort-Object -Unique localport  
rm  $Path  
$CSV = $CSV | Where-Object {[int]$_.localport -lt 49151}  
$CSV | Export-Csv $Path -NoClobber -NoTypeInformation  
