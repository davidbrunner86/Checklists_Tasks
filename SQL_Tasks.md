## auf WD Datenbank verbinden  

mit zb. MSSQL Mgmt Studio:  
np:\\.\pipe\MICROSOFT##WID\tsql\query  

Version anzeigen:  
Select @@version  


mit Powershell verbinden:  

Invoke-Sqlcmd -ServerInstance np:\\.\pipe\MICROSOFT##WID\tsql\query -Database master -Query "select name from sys.databases"  

