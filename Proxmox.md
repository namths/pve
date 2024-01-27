# Laptop doesn't suspend properly on closing lid

Edit logind.conf: `nano /etc/systemd/logind.conf`

`HandleLidSwitch=ignore`

`HandleLidSwitchExternalPower=ignore`

`HandleLidSwitchDocked=ignore`

# Edit sources.list to repo server Vietnam 

`nano /etc/apt/sources.list`

Change

`deb http://ftp.debian.org/debian bullseye main contrib` 

to `deb http://debian.xtdv.net/debian/ bullseye main contrib`

`deb http://ftp.debian.org/debian bullseye-updates main contrib` 

to `deb http://debian.xtdv.net/debian/ bullseye-updates main contrib`

# Disable message no subcription

Remove repository pve-enterprise `rm /etc/apt/sources.list.d/pve-enterprise.list`

Backup proxmoxlib.js: `cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak`

Edit proxmoxlib.js: `nano /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js`

Find (ctrl + w) `data.status` -> change condition =`if (false)`

# Enable iommu using and add PCI Device from host to vm

Edit grub: `nano /etc/default/grub`

Find line `GRUB_CMDLINE_LINUX_DEFAULT`: add end line `intel_iommu=on`, ex: `GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on”`

Update grub: `update-grub`

# Enable xterm-js

Proxmox VE -> Datacenter -> nodename -> VM -> Hardware -> Add -> SerialPort = `0`

Login VM:

Edit grub: `vi /etc/default/grub`

Find line `GRUB_CMDLINE_LINUX`: add end line `console=tty0 console=ttyS0,115200`, ex: `GRUB_CMDLINE_LINUX=“… console=tty0 console=ttyS0,115200”`

- Debian/Ubuntu/Kali Linux etc: `update-grub`

- RHEL/CentOS/Fedora: `grub2-mkconfig --output=/boot/grub2/grub.cfg`

# Auto login via xterm-js

`https://wiki.archlinux.org/title/Getty`

`systemctl edit serial-getty@ttyS0.service`
````
[Service]  
ExecStart=  
ExecStart=-/sbin/agetty --autologin root -s %I 115200,38400,9600 vt102
````
# Remove Directory in Proxmox

systemctl disable mnt-pve-testdir.mount

umount /mnt/pve/testdir

rm /etc/systemd/system/mnt-pve-testdir.mount

reboot

# Update/Upgrade Proxmox VE 7.x (PVE 7.x)

`nano /etc/apt/sources.list`

PVE pve-no-subscription repository provided by proxmox.com

NOT recommended for production use

add: `deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription`

`apt update && apt dist-upgrade`

# Modify/Change console/SSH login banner for Proxmox Virtual Environment (Proxmox VE / PVE)

Change file: `nano /usr/bin/pvebanner`

IP Node login: change file `nano /etc/hosts` line: `192.168.1.90 hp.local hp`

# Set Network to use DHCP

Change file: `nano /etc/hosts`

````
iface vmbr0 inet dhcp
    bridge-ports enp5s0
    bridge-stp off
    bridge-fd 0
````

Change hostname
$ `hostnamectl set-hostname pve.abc.com`

Dynamic Host Configuration
Add file: `nano /etc/dhcp/dhclient-exit-hooks.d/update-etc-hosts`

````
if ([ $reason = "BOUND" ] || [ $reason = "RENEW" ])
then
  sed -i "s/^.*\spve.abc.com\s.*$/${new_ip_address} pve.abc.com pve/" /etc/hosts
fi
````

# other: https://weblog.lkiesow.de/

