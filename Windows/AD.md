
## Quick Check AD Health:  

### DCDiag Script
DCDiag.cmd  

:start  
dcdiag /q  
repadmin /replsummary  
REM Script rennt ewig weiter. Einfach mit X beenden  
REM !Repl Fehler CLSID 0x00002720 koennen ignoriert werden!  
pause  
goto start  


## NTFRS Fehler 13568  

https://web.archive.org/web/20210517191458/https://blog.friedlandreas.net/2012/01/ntfrs-dateireplikationsdienst-protokolliert-den-fehler-13568/  

