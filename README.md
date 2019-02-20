# rtl8812au

Realtek 8812AU/8821AU USB WiFi driver.

for AC1200 (801.11ac) Wireless Dual-Band USB Adapter

Modification of https://github.com/abperiasamy/rtl8812AU_8821AU_linux with LED switched off

See original README at [ORIGINAL_README.md](ORIGINAL_README.md)

## Building

Needs building with the target kernel installed, on the desired architecture (e.g. Pi 1)

All done as root:

```bash
apt install raspberrypi-kernel-headers bc
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel-headers_1.20180924-1_armhf.deb # your kernel version
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel-headers_1.20180924-1_armhf.deb # your kernel version
dpkg -i *.deb
git clone https://github.com/astro-pi/rtl8812AU_8821AU_linux
cd rtl8812AU_8821AU_linux
ARCH=arm KVER=4.14.71+ make -j5
```

This outputs the driver `rtl8812.ko`

## Installing

```bash
cp rtl8812.ko /lib/modules/4.14.71+/kernel/drivers/net/wireless/ # your kernel version
depmod
modprobe rtl8812
```

Check it's installed correctly:

- Run `ls /sys/class/net` and you should see `eth0 lo wlan0`.

- Scan for WiFi networks:

```bash
iwlist scan
```

- Check `ifconfig` and you should see `wlan0` with no IP address

Connect to a network:

- Add your network config to `/etc/wpa_supplicant/wpa_supplicant.conf`

- Run `ifdown wlan0` and `ifup wlan0`

- Run `ifconfig` and you should have an IP address associated with `wlan0`
