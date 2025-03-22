#Debian Setup

normal install, mind 2 partitionen (sys+home) oder besser 3 (sys+var+home)  
login (desktop oder cli)  

:desktop  

apt update  
apt dist-upgrade  
apt full-upgrade  
reboot  

--gasterweiterungen  
sudo apt install -y build-essential linux-headers-$(uname -r)  
gasterweiterungen mounten in virtualbox  
sudo mount /dev/cdrom /mnt  
cd /mnt  
sudo ./VBoxLinuxAdditions.run  
sudo reboot  



--autologin (depending on display manager+env)  
	/etc/lightdm/lightdm.conf  
	[Seat:*]  
	autologin-user=meinuser  
	autologin-session=session  

reboot  

--jdownloader  
sudo apt update  
sudo apt install openjdk-17-jdk  
installiere jdownloader via sh script von homepage  


--sudoers user  
sudo usermod -aG sudo meinuser  
sudo visudo  
--Dieser Befehl öffnet die /etc/sudoers-Datei. Bei den Nutzer-Privilegien fügt man nun folgende Zeile hinzu:  
meinuser ALL=(ALL:ALL) ALL  


...nas mount  
... firefox restart tabs  
...jdbackup einspielen  
...?  


bei mate : programme einfach auf taskbar ziehen
