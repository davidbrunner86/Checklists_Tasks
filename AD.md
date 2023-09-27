
## Quick Check AD Health:  

# DCDiag Script
DCDiag.cmd  

:start  
dcdiag /q  
repadmin /replsummary  
REM Script rennt ewig weiter. Einfach mit X beenden  
REM !Repl Fehler CLSID 0x00002720 koennen ignoriert werden!  
pause  
goto start  
