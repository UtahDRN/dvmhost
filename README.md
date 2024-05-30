# UtahDRN dvmhost Installation

```
apt update
apt upgrade

apt install vim wget make cmake build-essentials libasio-dev libncurses-dev libssl-dev

wget https://github.com/DVMProject/dvmhost/archive/refs/heads/master.zip -O /tmp/dvmhost.zip
unzip /tmp/dvmhost.zip -d /opt/
mkdir -p /opt/dvmhost-master/build
cd /opt/dvmhost-master/build
cmake ..
make

mkdir -p /etc/dvmhost/bin /etc/dvmhost/configs

cp /opt/dvmhost-master/build/dvmhost /etc/dvmhost/bin/

curl -o /etc/systemd/system/multi-user.target.wants/dvmhost-cc.service https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/systemd/system/multi-user.target.wants/dvmhost-cc.service
curl -o /etc/systemd/system/multi-user.target.wants/dvmhost-vc.service https://raw.githubusercontent.com/UtahDRN/dvmhost/main/etc/systemd/system/multi-user.target.wants/dvmhost-vc.service

systemctl daemon-reload
systemctl enable dvmhost-cc
systemctl enable dvmhost-vc
```
