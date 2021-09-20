# Laptop doesn't suspend properly on closing lid

Edit logind.conf: `nano /etc/systemd/logind.conf`

`HandleLidSwitch=ignore`

`HandleLidSwitchExternalPower=ignore`

`HandleLidSwitchDocked=ignore`

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

# Auto login via xterm-js centos 7

`vi /lib/systemd/system/serial-getty@.service`

add `--autologin root` => `ExecStart=-/sbin/agetty --autologin root`

# Auto login via xterm-js ubuntu 20 lts

`https://wiki.archlinux.org/title/Getty`

`systemctl edit serial-getty@ttyS0.service`

`[Service]
ExecStart=
ExecStart=-/sbin/agetty --autologin root -s %I 115200,38400,9600 vt102`

# Remove Directory in Proxmox
systemctl disable mnt-pve-testdir.mount
umount /mnt/pve/testdir
rm /etc/systemd/system/mnt-pve-testdir.mount
reboot