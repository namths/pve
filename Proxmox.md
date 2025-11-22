# Laptop doesn't suspend properly on closing lid

Edit logind.conf: `nano /etc/systemd/logind.conf`

`HandleLidSwitch=ignore`

`HandleLidSwitchExternalPower=ignore`

`HandleLidSwitchDocked=ignore`

# Edit sources.list to repo server Vietnam 

`nano /etc/apt/sources.list.d/debian.sources`

Change

`https://debian.xtdv.net/debian/`

`https://mirror.twds.com.tw/debian-security/`

`adduser namths`
`usermod -aG sudo namths`

# Disable message no subcription

Remove repository pve-enterprise `rm /etc/apt/sources.list.d/pve-enterprise.list`

Backup proxmoxlib.js: `cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak`

Edit proxmoxlib.js: `nano /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js`

Find (ctrl + w) `data.status` -> change condition =`if (false)`

# Chỉnh sửa grub

Edit grub: `nano /etc/default/grub`

Edit `GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction initcall_blacklist=sysfb_init video=simplefb:off video=vesafb:off video=efifb:off video=vesa:off disable_vga=1 vfio_iommu_type1.allow_unsafe_interrupts=1 kvm.ignore_msrs=1 modprobe.blacklist=radeon,nouveau,nvidia,nvidiafb,nvidia-gpu,snd_hda_intel,snd_hda_codec_hdmi,i915"`

Update grub: `update-grub`

# Dành cho hệ thống zfs

`nano /etc/kernel/cmdline`

`quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction initcall_blacklist=sysfb_init video=simplefb:off video=vesafb:off video=efifb:off video=vesa:off disable_vga=1 vfio_iommu_type1.allow_unsafe_interrupts=1 kvm.ignore_msrs=1 modprobe.blacklist=radeon,nouveau,nvidia,nvidiafb,nvidia-gpu,snd_hda_intel,snd_hda_codec_hdmi,i915`

`proxmox-boot-tool refresh`

# Cấu hình PCI Passthrough

Edit modules: `nano /etc/modules`
Edit add:
```
# Modules required for PCI passthrough
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Run: `update-initramfs -u -k all`

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

Remove cluster
```
systemctl stop pve-cluster
systemctl stop corosync

pmxcfs -l

rm /etc/pve/corosync.conf
rm -r /etc/corosync/*

killall pmxcfs

systemctl start pve-cluster
```

