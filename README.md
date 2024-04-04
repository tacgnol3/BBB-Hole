# BBB-Hole


Credit goes to :

https://forum.beagleboard.org/t/debian-11-x-bullseye-monthly-snapshots/31280

https://www.reddit.com/r/pihole/comments/txaw23/pihole_on_beaglebone_black_the_uptodate_working/


I believe a simplified guide can be done.

For my example I used a BBB 2 gig, that doesn't have any wifi or bluetooth (AFAIK).

(Updated for debian 12 image)

Image :

[Minimal image](https://rcn-ee.com/rootfs/debian-armhf-12-bookworm-minimal-mainline/2024-04-04/am335x-debian-12.5-minimal-armhf-2024-04-04-2gb.img.bz2)

Newer/Older image can be found [here](https://rcn-ee.com/rootfs/debian-armhf-12-bookworm-minimal-mainline/).

I recommend Balena Etcher to write the image to the sd card.


First, to save some space, I recommend to disable the journal service since the free space is already low.

```shell
sudo systemctl disable systemd-journald.service
cd /var/log/journal/  # And then press TAB for the next folder since it seems to be a random long string
sudo rm *.journal
sudo reboot
```


Cleanup the already minimal install..

```shell
sudo apt autoremove -y bluez bb-u-boot-am5* bb-wlan0-defaults btrfs-progs firmware-atheros firmware-brcm80211 firmware-iwlwifi firmware-libertas firmware-realtek iw nginx* rsync vim* wireguard-tools wireless-regdb
```


Setup a static IP
```shell
sudo nano /etc/systemd/network/eth0.network
```

```shell
[Match]
Name=eth0
Type=ether

[Link]
RequiredForOnline=yes

[Network]
#DHCP=ipv4
Address=192.168.1.7/24  #Adjust for your setup
Gateway=192.168.1.1  #Adjust for your setup
DNS=8.8.8.8
```

Save & Exit with CTRL+O and CTRL+X

```shell
sudo Reboot
```


And finally install-pi-hole
```shell
curl -sSL https://install.pi-hole.net | bash
```

(optional) Finally, to install everything on the eMMC if you want:
```shell
sudo apt install bb-beagle-flasher
sudo enable-beagle-flasher
sudo reboot
```

Don't forget to change the default password with "passwd"

(Optional) If you want to turn off the user leds (the power leds is not affected), simply do this:
```shell
echo 0 > /sys/class/leds/beaglebone:green:usr0/brightness
echo 0 > /sys/class/leds/beaglebone:green:usr1/brightness
echo 0 > /sys/class/leds/beaglebone:green:usr2/brightness
echo 0 > /sys/class/leds/beaglebone:green:usr3/brightness
```
