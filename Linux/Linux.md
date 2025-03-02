## Move file preserving security info  

rsync -A --acls -X --xattrs -o --owner -g --group

mv --> preserves ownership  (if same filesystem / partition)
cp :  
cp -p file1 file2  

## listing users  

/etc/passwd  

ou can use getent passwd or cat /etc/passwd to access the passwd file  

getend passwd | grep username  


## adding a user  

useradd -m -s /bin/bash testuser  

## modifying user  

passwd username  

## deleting user password  

passwd -d username  

## locking a user  

passwd -l username  

## modifying the user primary group  

usermod -g groupname username  

## deleting a user  

userdel username  

## enablings password aging for a user

chage -M 20 username  
(20  days)  

## disabling password aging for a user  

chage -I -1 -E -1 -m 0 -M 99999 username  


## Dateien und Verzeichnisse auflisten in Shell  

du -sh *  
(du --summarize --human-readable *)  

du -sk * | sort -n  
(sort folders by size)  

du -sh * | sort -hr | head -n10  

source: https://stackoverflow.com/questions/1019116/using-ls-to-list-directories-and-their-total-sizes  


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


## Free disk space allgemein  

<ins>Speicherplatz anzeigen vom Apt Cache:</ins>  
sudo du -sh /var/cache/apt  

<ins>diesen löschen:</ins>  
sudo apt-get clean  

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


## Wireguard  

### via commandline WG tunnel installieren und starten  

nmcli connection import type wireguard file "/home/david/Dokumente/DavidWGLinuxHO.conf"  

nmcli connection up 'DavidWGLinuxHO'  
to be continued
