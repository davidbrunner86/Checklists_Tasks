## System klonen

<ins>Beispiel mit dd - backup von sda in ein Image</ins>  
dd if=/dev/sda of=/home/user/osbkp.img bs=1M status=progress

<ins>Image wiederherstellen auf sdb</ins>  
dd if=/home/user/osbkp.img of=/dev/sdb bs=1M status=progress

<ins>Beispiel live clone von sda auf sdb</ins>  
dd if=/dev/sda of=/dev/sdb bs=1M status=progress

<ins>Beispiel Partition klonen</ins>  
dd if=/dev/sda1 of=/home/user/part1.img bs=1M status=progress

im weiteren Verlauf besser mit rsync

<ins>Backup des gesamten Systems auf einen externen Datenträger, in dem Fall sdb1</ins>  
sudo mount /dev/sdb1 /mnt
sudo rsync -aAXv / --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /mnt


<ins>Wiederherstellen ist der rsync Befehl genau umgekehrt</ins>  

https://wiki.archlinux.org/title/rsync#Full_system_backup



## Free disk space / ref Citrix ADC  
https://docs.netscaler.com/en-us/citrix-adc/12-1/system/troubleshooting-citrix-adc/how-to-free-space-on-var-directory.html

meist nur in /var  

/var/nstrace - This directory contains trace files.This is the most common reason for HDD being filled on the Citrix ADC. This is due to an nstrace being left running for indefinite amount of time. All traces that are not of interest can and should be deleted. To stop an nstrace, go back to the CLI and issue stop nstrace command.  
/var/log - This directory contains system specific log files.  
/var/nslog - This directory contains Citrix ADC log files.  
/var/tmp/support - This directory contains technical support files, also known as, support bundles. All files not of interest should be deleted.  
/var/core - Core dumps are stored in this directory. There will be directories within this directory and they will be labeled with numbers starting with 1. These files can be quite large in size. Clear all files unless the core dumps are recent and investigation is required.  
/var/crash - Crash files, such as process crashes are stored in this directory. Clear all files unless the crashes are recent and investigation is required.  
/var/nsinstall -  Firmware is placed in this directory when upgrading. Clear all files, except the firmware that is currently being used.  

anzeigen:  
du -hs *  

lösche unbenötigte Dateien:  
rm -r nstrace/*  


https://ss64.com/bash/


to be continued
