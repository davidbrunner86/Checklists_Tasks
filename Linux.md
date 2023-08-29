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


to be continued
