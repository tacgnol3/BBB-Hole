# BBB-Hole


Credit goes to :

https://forum.beagleboard.org/t/debian-11-x-bullseye-monthly-snapshots/31280

https://www.reddit.com/r/pihole/comments/txaw23/pihole_on_beaglebone_black_the_uptodate_working/


I believe a simplified guide can be done.

For my example I used a BBB 2 gig, that doesn't have any wifi or bluetooth (AFAIK).


Image :

https://rcn-ee.com/rootfs/bb.org/testing/2023-05-03/bullseye-minimal-armhf/am335x-debian-11.7-minimal-armhf-2023-05-03-2gb.img.xz

Cleanup the already minimal install..

sudo apt autoremove bluez bb-wlan0-defaults firmware-iwlwifi firmware-atheros rsync wireguard-tools vim* bb-u-boot-am5* bb-cape-overlays 1firmware-libertas firmware-realtek firmware-brcm80211 firmware-zd1211 firmware-realtek btrfs-progs wpasupplicant iw


Setup a static IP

sudo nano /etc/systemd/network/eth0.network

[Match]
Name=eth0
Type=ether

[Link]
RequiredForOnline=yes

[Network]
#DHCP=ipv4
Address=192.168.1.7/24
Gateway=192.168.1.7
DNS=8.8.8.8


Save & Exit

Reboot



And finally install-pi-hole

curl -sSL https://install.pi-hole.net | bash


If you want to turn off the user leds (the power leds is not affected), simply do this:

echo 0 > /sys/class/leds/beaglebone:green:usr0/brightness

echo 0 > /sys/class/leds/beaglebone:green:usr1/brightness

echo 0 > /sys/class/leds/beaglebone:green:usr2/brightness

echo 0 > /sys/class/leds/beaglebone:green:usr3/brightness
