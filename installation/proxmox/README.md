# Installation of Proxmox 6.2 cluster with GPU virtualization support #

## Hardware Requirements ##
* 2 PCs: PC1 and PC2 (PC2 has a NVIDIA GPU)
* PC2 must have CPU and GPU virtualization enabled in BIOS (Virt-x or Virt-D and IOMMU + ACS for grouping PCI devices in dedicated groups !)
* each PC should have one dedicated OS disk and one or more SSDs for Image data

## Installation ##

### Proxmox installer ###
* Download installer ISO
* chose OS disk and install, reboot
* Add package sources for installation without subscription:

  File /etc/apt/sources.list:

  ```
  deb http://ftp.debian.org/debian buster main contrib
  deb http://ftp.debian.org/debian buster-updates main contrib

  # PVE pve-no-subscription repository provided by proxmox.com,
  # NOT recommended for production use
  deb http://download.proxmox.com/debian/pve buster pve-no-subscription

  # security updates
  deb http://security.debian.org/debian-security buster/updates main contrib
  ```

  ```
  apt update
  apt upgrade
  reboot
  ```

## Preparations for GPU ##

* Blacklist nouveau driver:
  
  In /etc/modprobe.d/blacklist-nouveau.conf :
  ```
  echo blacklist nouveau > /etc/modprobe.d/blacklist-nouveau.conf
  ```

* find the PCI ids for your GPU with: 
  ```
  lspci -nnk
  ```

  Add vfio support for them with:
  ```
  echo "options vfio-pci ids=10de:13c2,10de:0fbb" > /etc/modprobe.d/vfio.conf
  ```

### Create thin pool for /var/lib/vz ###

```
# Create partition on new disk
sgdisk -N 1 /dev/sdb 

# Create a Physical Volume (PV) without confirmation and 250K metadatasize.
pvcreate --metadatasize 250k -y -ff /dev/sdb1

# Create a volume group named “vmdata” on /dev/sdb1
vgcreate vmdata /dev/sdb1

# Creating an extra LV for /var/lib/vz
# This can be easily done by creating a new thin LV.
# lvcreate -n <Name> -V <Size[M,G,T]> <VG>/<LVThin_pool>
lvcreate -n vz -V 1T 
Now a filesystem must be created on the LV.

# mkfs.ext4 /dev/pve/vz

lvcreate -L 900G -T -n vmstore vmdata
```


```
service pvedaemon stop
service pveproxy stop
service pvestatd stop
service pve-ha-crm stop
service pve-ha-lrm stop
```

```
rm -rf /var/lib/vz
mkdir /var/lib/vz
echo '/dev/vmdata/vmstore /var/lib/vz ext4 defaults 0 2' >> /etc/fstab
``` 




### Add Gnome desktop (optional) ###





Add user:

```
adduser sepp
```

Add user to sudoers:

```






