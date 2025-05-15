# Diverse DISM und Reparatur Tasks  

#Wenn ein update fehlgeschlagen ist. In Wiederherstellungsumgebung oder von Install CD booten und dann in cmd  
dism.exe /image:C:\ /cleanup-image /revertpendingactions  


Befehle in OS:  
netsh winsock reset  
netsh winsock reset proxy  

rundll32.exe pnpclean.dll,RunDLL_PnpClean /DRIVERS /MAXCLEAN  

dism /Online /Cleanup-image /ScanHealth  
dism /Online /Cleanup-image /CheckHealth  
dism /Online /Cleanup-image /RestoreHealth  

# Bei problemen mit einem Host ggf. Reparatur über anderen Host  
DISM /Online /Cleanup-Image /RestoreHealth /Source:\\[SERVER]\C$\windows  


DISM /cleanup-image /online /analyzecomponentstore  

dism /Online /Cleanup-image /StartComponentCleanup  

DISM /cleanup-image /online /startcomponentcleanup /resetbase  

Sfc /ScanNow  


 
# Export Treiber  
###exportiert Driver in einen Ordner  

dism /online /export-driver /destination:"C:\Users\%Username%\Desktop\driverbackup"  

