# VMDK vHDD konvertieren
#VMDK in VHD mit Virtualbox VBoxManage konvertieren (für Hyper-v)

C:\Program Files\Oracle\VirtualBox>VBoxManage.exe clonehd “C:\Kali-Linux-2018.2-vbox-amd64\Kali-Linux-2018.2-vbox-amd64-disk001.vmdk” e:\temp\kali.vhd –format VHD  

# VHDX zu VDI konvertieren  
 cd 'C:\Program Files\Oracle\VirtualBox\'  
.\VBoxManage.exe clonemedium disk "F:\HOMEVPN_Lenovo.VHDX" "D:\VMs\Shiva11_alt\lenovo_mini-home.VDI" --format vdi  
