# Diverse CMDlets im Exchange Server - aktuell getestet für Exchange 2019  

#Warteschlange ansehen:  
Get-Queue  | ft  

#Manuell Logs archivieren (sollte eigentlich ein Task der Aufgabenplanung sein)  
powershell.exe -command "C:\scripts\IISLogsCleanup.ps1 -LogPath 'D:\Inetpub\logs\LogFiles\W3SVC1'"  
powershell.exe -command "C:\scripts\IISLogsCleanup.ps1 -LogPath 'D:\Inetpub\logs\LogFiles\W3SVC2'"  


#Moverequests:  
get-moverequest -MoveStatus InProgress | Get-MoveRequestStatistics | select Displayname,percentcomplete  
Get-MoveRequest | where{$_.Status -ne "Completed"} | Get-MoveRequestStatistics | select Displayname,percentcomplete  

get-mailbox "chuck.norris" | new-moverequest -TargetDatabase DB01  


#Verbundene Clients abfragen:  
Get-NetTCPConnection | where {(($_.localport -eq 443) -and ($_.remoteport -ne 0))} | select LocalAddress,RemoteAddress,RemotePort,State,CreationTime | out-gridview  
( quelle: https://www.msxfaq.de/exchange/tools/auswertung/tcpconnection_report.htm )  


#AMSI deaktivieren:  
New-SettingOverride -Name "DisablingAMSIScan" -Component Cafe -Section HttpRequestFiltering -Parameters ("Enabled=False") -Reason "Testing"  
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh  
Restart-Service -Name W3SVC, WAS -Force  

#AMSI aktivieren:  
Remove-SettingOverride -Name "DisablingAMSIScan" -Component Cafe -Section HttpRequestFiltering -Parameters ("Enabled=False") -Reason "Testing"  
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh  
Restart-Service -Name W3SVC, WAS -Force  


New-MailboxDatabase -Server MeinMailServer -Name "DBPublicFolder" -EdbFilePath F:\DBPublicFolder\DBPublicFolder.edb  
Move-DatabasePath DBPublicFolder -EdbFilePath d:\DBPublicFolder\DBPublicFolder.edb -LogFolderPath d:\DBPublicFolder  


#Gelöschte Elemente der Mailboxen  
Get-Mailbox -database DB01 -resultsize unlimited | Get-MailboxFolderStatistics | ? {$_.FolderPath -eq "/Deleted Items"} | select Identity, FolderAndSubFolderSize  


#Getrennte Mailboxen auflisten  
Get-MailboxDatabase | Get-MailboxStatistics | Where { $_.DisconnectReason -eq "SoftDeleted" } | ft DisplayName,Database,DisconnectDate,MailboxGUID,DisconnectReason  

#Einzelne getrennte Mailboxen entfernen (braucht obigen Befehl)  
Remove-StoreMailbox -Database DBPublicFolder -Identity "03f3ee63-3029-4821-a5a8-a00cd7460e55" -MailboxState SoftDeleted  
( löscht die Mailbox in der genannten Datenbank mit der ID im Status SoftDeleted  )


#Anzahl Mailboxen (alle) pro DB zählen  
Get-Mailbox -ResultSize Unlimited -Database "DB01" | Group-Object -Property:Database | Select-Object Name, Count | Format-Table  


#Mailbox-Sizes  

$Mailboxsizes = Get-MailboxStatistics -Server giatechbrmail | Select-Object DisplayName, @{Name="TotalItemSizeMB"; Expression={[math]::Round(($_.TotalItemSize.ToString().Split("(")[1].Split(" ")[0].Replace(",","")/1MB),0)}}, ItemCount | Sort-Object TotalItemSizeMB -Descending  

$Mailboxdeletedfoldersizes = $AllMailboxes | foreach {Get-MailboxFolderStatistics -Identity $_.Identity -FolderScope DeletedItems | select Identity, FolderAndSubfolderSize | Sort-Object -Property FolderAndSubfolderSize } | Where-Object FolderAndSubfolderSize -like *GB*  

$Mailboxsentfoldersizes = $AllMailboxes | foreach {Get-MailboxFolderStatistics -Identity $_.Identity -FolderScope SentItems | select Identity, FolderAndSubfolderSize | Sort-Object -Property FolderAndSubfolderSize } | Where-Object FolderAndSubfolderSize -like *GB*  


$AllMailboxes = Get-Mailbox -ResultSize Unlimited  

$Mailboxbigfoldersizes = $AllMailboxes | foreach {Get-MailboxFolderStatistics -Identity $_.Identity | select Identity, FolderAndSubfolderSize | Sort-Object -Property FolderAndSubfolderSize } | Where-Object FolderAndSubfolderSize -like *GB*  

#ReceiveConnector anpassen, Anmeldung  
Get-ReceiveConnector "Anonymes Relay" | Add-ADPermission -User "NT-Autorität\Anonymous-Anmeldung" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
