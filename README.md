# UtahDRN dvmhost Installation

### Base System Changes
```
sudo rpi-update
apt update
apt upgrade

apt install vim wget curl make cmake build-essential libasio-dev libncurses-dev libssl-dev
```

### Build and Install dvmhost
```
wget https://github.com/DVMProject/dvmhost/archive/refs/heads/master.zip -O /tmp/dvmhost.zip
unzip /tmp/dvmhost.zip -d /opt/
mkdir -p /opt/dvmhost-master/build
cd /opt/dvmhost-master/build
cmake ..
make

mkdir -p /etc/dvmhost/configs /var/log/dvmhost

cp /opt/dvmhost-master/build/dvmhost /etc/dvmhost/
```

### Create Service
```
curl -o /lib/systemd/system/dvmhost-cc.service https://raw.githubusercontent.com/UtahDRN/dvmhost/main/lib/systemd/system/dvmhost-cc.service
curl -o /lib/systemd/system/dvmhost-vc.service https://raw.githubusercontent.com/UtahDRN/dvmhost/main/lib/systemd/system/dvmhost-vc.service

systemctl daemon-reload
systemctl enable dvmhost-cc
systemctl enable dvmhost-vc
```

### Cleanup
```
rm -rf cd /opt/dvmhost-master/build
```

### Update Firmware
From USB
```
stm32flash -v -w dvm-firmware-hs_f1.bin -g 0x0 -R /dev/ttyUSB0
``` 

From Pi

First you will need to disable the serial console and disable bluetooth. Edit ```/boot/firmware/cmdline.txt``` and remove the following ```console=serial0,115200 console=tty1```.
Next, you will need to disable bluetooth on the board. Edit ```/boot/firmware/config.txt``` and add a line containing ```dtoverlay=disable-bt``` after the [All] section at the bottom. Reboot.

> Most sets of instructions reccomend to download stm32flash from online, however we have found the prepackaged version to work fine.

Once the hotspot is back on, navigate to the build folder where you compiled the firmware. Put a jumper across the J1 points on the board, and the RED heartbeat LED should stop flashing. Run the below command to flash.

```
stm32flash -v -w dvm-firmware-hs_f1.bin -i 20,-21,21,-20 -R /dev/ttyAMA0
```

### Download Configs
```
curl -o /etc/dvmhost/configs/iden_table.dat https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/dvmhost/configs/iden_table.dat
curl -o /etc/dvmhost/configs/talkgroup_rules.yml https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/dvmhost/configs/talkgroup_rules.yml
curl -o /etc/dvmhost/configs/utahdrn-cc1.yml https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/dvmhost/configs/utahdrn-cc1.yml
curl -o /etc/dvmhost/configs/utahdrn-vc1.yml https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/dvmhost/configs/utahdrn-vc1.yml
```

### Start Service
```
systemctl start dvmhost-cc
systemctl start dvmhost-vc
```

## Useful Links
https://github.com/DVMProject/iden-channel-calculator

https://dvmproject.io/iden-calc-web/

Downlink = Repeater TX / Portable RX

Uplink = Repeater RX / Portable TX
