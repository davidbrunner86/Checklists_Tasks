# um lästiges McAfee los zu werden ...  

Bei Agent Problemen:  

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\McAfeeFramework  
Double-click the Start value and set the Value data to 4.  
reboot  
frminst.exe /remove=agent  

Und dann MCPR.exe  

default password ggf. McAfee  

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services  
mfecw löschen  

c:\prog.... mcafee löschen  
