# Temporäres Verzeichnis erstellen
mkdir -p /tmp/rtl8812bu-driver && cd /tmp/rtl8812bu-driver

# Abhängigkeiten installieren
sudo apt update
sudo apt install -y dkms git build-essential linux-headers-$(uname -r) git rsync dkms
sudo apt install make

# Repository klonen
git clone https://github.com/cilynx/rtl88x2bu.git
cd rtl88x2bu

# Treiber via DKMS installieren
sudo ./deploy.sh

make
sudo make install

# Modul laden
sudo modprobe 88x2bu

# Falschen Treiber (rtl8xxxu) blockieren
echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/blacklist-rtl8xxxu.conf
sudo update-initramfs -u

# Neustart, damit Änderungen wirksam werden
echo "Installation abgeschlossen. Bitte jetzt den Rechner neu starten mit: sudo reboot"


# USB Devies prüfen:
lsusb

# Treiber Fehler prüfen:
dmesg | grep -i firmware

# IF prüfen
ip a
ip link
nmcli device status
nmcli device wifi list

lsmod | grep 8812au
lsmod | grep 88x2bu

# Hilfreiches:
#zeigt, wo DKMS liegt. vermutlich in /usr/sbin
dpkg -L dkms | grep bin

#wenn /usr/sbin nicht im PATH enthalten ist, wird dkms vermutlich nicht gefunden
echo $PATH

#temporär in $PATH hinzufügen:
export PATH=$PATH:/usr/sbin
