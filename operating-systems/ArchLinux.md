# Arch Linux

## Basics

| Command                 | Action             |
|-------------------------|--------------------|
| `pacman -S <package>`   | Install package    |
| `pacman -R <package>`   | Remove package     |
| `pacman -Ss <query>`    | Search package     |
| `pacman -Syu`           | Update packages    |

## Arch User Repository (AUR)

User-provided software, available in https://aur.archlinux.org/index.php

### Install packages manually

1. Download the tarball
2. Extract it, go to the folder  
  `tar -xvf package_name.tar.gz -C dirname`  
  `cd dirname`
3. Create package and install it  
  `makepkg -sri`

If makepkg complains about PGP keys (ex: `1EB2638FF56C0C53`):  
`gpg --recv-keys --keyserver hkp://pgp.mit.edu 1EB2638FF56C0C53`

To install using root, use su (not recommended):  
`su -s /bin/bash nobody`

### List AUR packages
`pacman -Qm`

### Install packages automatically
```bash
cd aur
cower -s <name>           # search
cower -d <name>           # download
cd <name>
makepkg -sri
```

#### Update packages automatically
`cower -duf --timeout=20`

#### Suggested packages
- broadcom-wl-dkms
- bcwc-pcie-firmware
- bcwc-pcie-dkms

## Installation guide (2016-03-03)

### First steps

Burn the ISO to a USB  
`dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync`

List available keyboard layouts  
`ls /usr/share/kbd/keymaps/**/*.map.gz`

Change keyboard layout  
`loadkeys pt-latin1       # or: loadkeys mac-pt-latin1`

Optionally change the console font  
`setfont Lat2-Terminus16`

Enable wired connection
```
ip link                                       # to find out <interface>
systemctl enable dhcpcd@<interface>.service
dhcpcd
```

Configure Wireless  
`wifi-menu`

Start time sync and check status
```bash
timedatectl set-ntp true
timedatectl status
```

### Setup disk

Check disks  
`lsblk                           # or: fdisk -l`

Check current partition table  
`parted /dev/sdx print`

#### Swap  Partition

##### Partitioning using MBR, / + swap
**Note**: This bricked an Acer's HDD, maybe because Swap comes first or because lack of formatting
```bash
parted /dev/sdx
mklabel msdos
mkpart primary linux-swap 1MiB 6GiB
mkpart primary ext4 6GiB 100%
set 2 boot on
quit
```

##### Partitioning using EFI, / + swap (Macbook)
Remember to allocate 128MB in an HFS+ (af00) partition for a Linux Boot Loader
```bash
cgdisk /dev/sdx
Linux boot loader 128 Mib
swap 4200 Mib
root Remaining Space
Type codes: Ext4 is 8300 (the default), Linux swap is 8200
```

Check partitions  
`lsblk /dev/sdx`

Format ext4 partitions  
`mkfs.ext4 /dev/sdxY`

Activate swap
```bash
mkswap /dev/sdxY
swapon /dev/sdxY
```

Mount root partition  
`mount /dev/sdxY /mnt`

