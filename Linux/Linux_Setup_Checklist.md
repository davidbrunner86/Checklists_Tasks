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
