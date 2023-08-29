## System klonen

Beispiel mit dd - backup von sda in ein Image
dd if=/dev/sda of=/home/user/osbkp.img bs=1M status=progress

Image wiederherstellen auf sdb
dd if=/home/user/osbkp.img of=/dev/sdb bs=1M status=progress

Beispiel live clone von sda auf sdb
dd if=/dev/sda of=/dev/sdb bs=1M status=progress

Beispiel Partition klonen
dd if=/dev/sda1 of=/home/user/part1.img bs=1M status=progress

im weiteren Verlauf besser mit rsync

Backup des gesamten Systems auf einen externen Datenträger, in dem Fall sdb1
sudo mount /dev/sdb1 /mnt
sudo rsync -aAXv / --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /mnt


Wiederherstellen ist der rsync Befehl genau umgekehrt

https://wiki.archlinux.org/title/rsync#Full_system_backup


to be continued
