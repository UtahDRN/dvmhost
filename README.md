# UtahDRN dvmhost Installation

### Base System Changes
```
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
```
stm32flash -v -w dvm-firmware-hs_f1.bin -g 0x0 -R /dev/ttyUSB0
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
