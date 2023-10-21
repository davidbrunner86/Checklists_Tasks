# Sammlung für den Raspi

### automatisch mit Wlan verbinden nach Installation  

auf der boot Partition (eine FAT32 Partition, teils erst nach dem erstellen mit W32 Disk Imager gemountet)  
eine Datei *wpa_supplicatn.conf* anlegen  

Inhalt:  

ctrl_interface=DIR=/var/run/wpa_supplicant  

network={  
 scan_ssid=1  
 ssid="MyNetworkSSID"  
 psk="Pa55w0rd1234"  
}  

(ssid und psk durch eigene Werte ersetzen)  

Beispiel:  
https://www.raspberrypi-spy.co.uk/2017/04/manually-setting-up-pi-wifi-using-wpa_supplicant-conf/  

### SSH aktivieren  

eine leere Datei auf der boot partition mit den Namen *ssh* anlegen
