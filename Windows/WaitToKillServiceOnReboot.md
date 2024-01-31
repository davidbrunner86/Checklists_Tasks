### Registry Key to set Windows to wait for services to properly stop before killing them and reboot
## Setting for 30 Seconds

Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control]

"WaitToKillServiceTimeout"="30000"
