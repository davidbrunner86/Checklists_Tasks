net stop wuauserv  
net stop cryptSvc  
net stop bits  
net stop msiserver  
cd %systemroot%  
rename SoftwareDistribution SoftwareDistribution.old  
Ren C:\Windows\System32\catroot2 Catroot2.old  
rmdir /q /s c:\windows\temp  
net stop trustedinstaller  


c:  
cd c:\windows\logs\CBS  
del *.cab  
del *.log  
rem regenerate cab files  
net start wuauserv  
net start cryptSvc  
net start bits  
net start msiserver  
c:\windows\system32\wuauclt.exe /detectnow  
net start wuauserv  
echo this is a dummy line  
