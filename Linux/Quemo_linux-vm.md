Beispiele zum starten:  
"C:\Program Files\qemu\qemu-system-x86_64" -hda d:\debian_squeeze_amd64_standard.qcow2 -nic user,model=virtio-net-pci -redir tcp:2222::22  

"C:\Program Files\qemu\qemu-system-x86_64" -hda D:\debian-10-openstack-amd64.qcow2 -net user -redir tcp:2222::22  


qemu-system-x86_64 -hda d:\debian_squeeze_amd64_standard.qcow2 -cdrom .\ -m 1024 -net nic,model=virtio -net user -vga std -boot strict=on  

qemu-system-x86_64 -boot d -cdrom .\debian-10.7.0-amd64-netinst.iso -hda d:\debian_squeeze_amd64_standard.qcow2 -m 1024 -net nic,model=virtio -net user -vga std -boot strict=on  

sources:  
deb http://ftp.at.debian.org/debian unstable main contrib non-free  
deb-src http://ftp.at.debian.org/debian unstable main contrib non-freeapt au  

#FUNKTIONAL  
qemu-system-x86_64 -boot d -hda d:\debian_squeeze_amd64_standard.qcow2 -m 1024 -net nic,model=virtio -smp 2 -net user -vga std -boot strict=on  
########  

-enable-kvm -nographic -nodefaults -monitor stdio -serial pty -M pseries -smp 1,cores=1,threads=8 -m 4G -device spapr-vlan,netdev=net0,mac=4c:45:42:45:02:02 -netdev bridge,id=net0,br=br0  


qemu-system-x86_64 -boot d -hda d:\debian_squeeze_amd64_standard.qcow2 -m 1024 -smp 1 -net user -netdev user,id=net0,hostfwd=tcp::2222-:22 -vga std -boot strict=on  


### Experimentell funktional mit fehlermeldung "warning: hub 0 with no nics"  
qemu-system-x86_64 -boot c -hda d:\debian_squeeze_amd64_standard.qcow2 -m 1024 -smp 2 -net user -nic user,hostfwd=tcp::2222-:22,hostfwd=tcp::33389-:3389 -vga std -boot strict=on  
###  







-boot c = disk
-boot d = cd/dvd


commands:  
apt install xrdp  
apt install qemu-guest-agent  

nano /etc/ssh/sshd_config  


change to  
PermitRootLogin yes  

save  

# /etc/init.d/ssh restart  

