#!/bin/bash
sudo su -c "apt-get update"
sudo su -c "apt-get -y --force-yes install mc vim python-pip python-dev python-setuptools python-virtualenv git libyaml-dev build-essential"

mkdir $HOME/opt && cd opt
git clone https://github.com/foosel/OctoPrint.git && cd OctoPrint
virtualenv venv
./venv/bin/pip install pip --upgrade
./venv/bin/python setup.py install
mkdir $HOME/opt/.op{01..02}

rm $HOME/opt/OctoPrint/scripts/op1 \
$HOME/opt/OctoPrint/scripts/op2 \
$HOME/opt/OctoPrint/scripts/op1.init \
$HOME/opt/OctoPrint/scripts/op2.init

cp octoprint.default op1
cp octoprint.default op2
cp octoprint.init op1.init
cp octoprint.init op2.init

#1 octoprint.default changes
sed -i -e "s|.*BASEDIR=.*|BASEDIR=$HOME/opt/.op1|" ~/opt/OctoPrint/scripts/op1
sed -i -e "s|.*CONFIGFILE=.*|CONFIGFILE=$HOME/opt/.op1/config.yaml|" ~/opt/OctoPrint/scripts/op1
sed -i -e "s|.*PORT=.*|PORT=5000|" ~/opt/OctoPrint/scripts/op1
sed -i -e "s|.*DAEMON=.*|DAEMON=$HOME/opt/OctoPrint/venv/bin/octoprint|" ~/opt/OctoPrint/scripts/op1

#1 octoprint.init changes
sed -i -e "s|# Provides:          octoprint|# Provides:          op1|" ~/opt/OctoPrint/scripts/op1.init
sed -i -e "s|# Short-Description: OctoPrint daemon|# Short-Description: OctoPrint 1 daemon|" ~/opt/OctoPrint/scripts/op1.init
sed -i -e "s|DESC=\"OctoPrint Daemon\"|DESC=\"OctoPrint 1 Daemon\"|" ~/opt/OctoPrint/scripts/op1.init
sed -i -e "s|NAME=\"OctoPrint\"|NAME=\"OctoPrint1\"|" ~/opt/OctoPrint/scripts/op1.init
sed -i -e "s|PKGNAME=octoprint|PKGNAME=op1|" ~/opt/OctoPrint/scripts/op1.init

# install and start service
sudo su -c "cp /home/pi/opt/OctoPrint/scripts/op1 /etc/default/op1"
sudo su -c "cp /home/pi/opt/OctoPrint/scripts/op1.init /etc/init.d/op1"
sudo su -c "chmod +x /etc/init.d/op1"
sudo su -c "update-rc.d op1 defaults"
sudo su -c "service op1 start"

#2 octoprint.default changes
sed -i -e "s|.*BASEDIR=.*|BASEDIR=$HOME/opt/.op2|" ~/opt/OctoPrint/scripts/op2
sed -i -e "s|.*CONFIGFILE=.*|CONFIGFILE=$HOME/opt/.op2/config.yaml|" ~/opt/OctoPrint/scripts/op2
sed -i -e "s|.*PORT=.*|PORT=5001|" ~/opt/OctoPrint/scripts/op2
sed -i -e "s|.*DAEMON=.*|DAEMON=$HOME/opt/OctoPrint/venv/bin/octoprint|" ~/opt/OctoPrint/scripts/op2

#2 octoprint.init changes
sed -i -e "s|# Provides:          octoprint|# Provides:          op2|" ~/opt/OctoPrint/scripts/op2.init
sed -i -e "s|# Short-Description: OctoPrint daemon|# Short-Description: OctoPrint 2 daemon|" ~/opt/OctoPrint/scripts/op2.init
sed -i -e "s|DESC=\"OctoPrint Daemon\"|DESC=\"OctoPrint 2 Daemon\"|" ~/opt/OctoPrint/scripts/op2.init
sed -i -e "s|NAME=\"OctoPrint\"|NAME=\"OctoPrint2\"|" ~/opt/OctoPrint/scripts/op2.init
sed -i -e "s|PKGNAME=octoprint|PKGNAME=op2|" ~/opt/OctoPrint/scripts/op2.init

# install and start service
sudo su -c "cp /home/pi/opt/OctoPrint/scripts/op2 /etc/default/op2"
sudo su -c "cp /home/pi/opt/OctoPrint/scripts/op2.init /etc/init.d/op2"
sudo su -c "chmod +x /etc/init.d/op2"
sudo su -c "update-rc.d op2 defaults"
sudo su -c "service op2 start"

# allow pi user to use serial port
sudo su -c "usermod -a -G tty pi"
sudo su -c "usermod -a -G dialout pi"

# allow pi user to run shutdown and service commands
sudo su -c "echo \"pi ALL=NOPASSWD: /sbin/shutdown\" > /etc/sudoers.d/octoprint-shutdown"
sudo su -c "echo \"pi ALL=NOPASSWD: /sbin/service\" > /etc/sudoers.d/octoprint-service"

# Fix SSH
sudo su -c "echo \"IPQoS 0x00\" >> /etc/ssh/sshd_config"
