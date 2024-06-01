# Debian Base verwenden  

#Template (CLI) auf einem Hypervisor zb.  

- hostname ändern
	sudo hostnamectl set-hostname BEISPIEL  
	
- unattended upgrade konfigurieren  
- NTP konfigurieren  

	apt-get install ntp  
	systemctl restart ntp  

- IP vergeben (statisch statt DHCP)  

	https://www.fosslinux.com/62880/how-to-set-up-a-static-ip-address-on-debian-11.htm  

- Bei Virtualbox: Guest-Additions installieren:  
  	echo deb http://deb.debian.org/debian/ sid main contrib non-free > /etc/apt/sources.list  
	apt update  
	apt install virtualbox-guest-x11  
	reboot  
