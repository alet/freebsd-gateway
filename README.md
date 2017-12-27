# FreeBSD-Gateway
Ansible configuration script FreeBSD based gateway for SOHO

### Prerequires
1. Server should be setuped on ZFS.
2. The server name should be setuped.
3. Python should be installed.
4. LAN interfaces are bridged.
5. Root remote login configured at least 'without-password'

The playbook needs sysrc module from <https://github.com/alet/ansible-sysrc>

### Installation
```sh
ssh root@gateway hostname gateway
ssh root@gateway pkg install python
ssh root@gateway cat <<EOF >>/etc/rc.conf
cloned_interfaces="bridge0 tap0"
ifconfig_bridge0="inet 192.168.5.1/24 addm wlan0 addm bge0 addm tap0 up"
ifconfig_bge0="up"
ifconfig_tap0="up"
EOF

sudo pip install ansible
git clone git@github.com:alet/freebsd-gateway.git
cd freebsd-gateway/
mkdir library
wget -o library/sysrc https://raw.githubusercontent.com/alet/ansible-sysrc/master/library/sysrc
cat <<EOF > stage
[gateway]
192.168.5.1
EOF
ansible-playbook --ask-become-pass -b -i stage site.yml
```

### Postinstall

1. You may want configure root mail's redirection to your box.
2. Run rkhunter --propupd
