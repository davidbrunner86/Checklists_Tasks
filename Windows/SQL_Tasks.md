## auf WD Datenbank verbinden  

mit zb. MSSQL Mgmt Studio:  
np:\\.\pipe\MICROSOFT##WID\tsql\query  

Version anzeigen:  
Select @@version  


mit Powershell verbinden:  

Invoke-Sqlcmd -ServerInstance np:\\.\pipe\MICROSOFT##WID\tsql\query -Database master -Query "select name from sys.databases"  

#verbinden mit lokaler wsus db  
np:\\.\pipe\MSSQL$MICROSOFT##SSEE\sql\query  

#WSUS Inventory Abfrage:  
select * from dbo.tbcomputertarget join dbo.tbcomputertargetdetail on dbo.tbcomputertarget.TargetID = dbo.tbcomputertargetdetail.TargetID  


## remap user nach Migration  

fehler:  
User, group, or role '{login}' already exists in the current database.  

lösung:  
USE {database};  
ALTER USER {user} WITH login = {login}  

## simple PowerShell SQL (simple) backup  

Backup-SqlDatabase -ServerInstance "SERVER\INSTANCE" -Database "MyDBNAme" -BackupFile "D:\SQL_Backup\Backup-myDB.bak"  

#scheduled Task  
user:  
timer variabel  
Action: PowerShell.exe ,  


#how to trace who is using SQL db  

# neue Instanz SQL Server zusätzlich  

einfach nochmal Installation und "perform a new Installation" und dann halt named instance  


#sql fw regeln  
New-NetFirewallRule -DisplayName "SQLServer default instance" -Direction Inbound -LocalPort 1433 -Protocol TCP -Action Allow  
New-NetFirewallRule -DisplayName "SQLServer Browser service" -Direction Inbound -LocalPort 1434 -Protocol UDP -Action Allo  
oder  
netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  

#find SQL Server running port  
PID von SQLServer via Taskmanager herausfinden  
netstat -ano | findstr *PID*  
