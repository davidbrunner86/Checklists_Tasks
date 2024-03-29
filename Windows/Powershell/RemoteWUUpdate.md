source = https://serverfault.com/questions/645604/howto-initiate-windows-update-on-server-core-from-a-ps-remote-session  

Get the pswidowsupdate files from the web & unzip them. Then import the module & run this code (the invoke-wsuinstall.ps1 file has the sample code but I removed a bit from it and it still works):  

$Script = {Get-WUInstall -AcceptAll -AutoReboot | Out-File C:\PSWindowsUpdate.log}  
Invoke-WUInstall -ComputerName computername -Script $Script  
Get-Content \ \ computername\c$\PSWindowsUpdate.log  
