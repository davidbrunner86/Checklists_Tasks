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
