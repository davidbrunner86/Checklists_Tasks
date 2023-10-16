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


## Diverse  

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
