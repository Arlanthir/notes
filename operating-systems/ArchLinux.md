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


### Installation

Update mirror list with preferred mirrors on the top:
```bash
nano /etc/pacman.d/mirrorlist
```
(To cut two lines, press Ctrl+K two times in a row)

Install:
```bash
pacstrap -i /mnt base base-devel
```

Generate fstab:
```bash
# genfstab -U /mnt >> /mnt/etc/fstab
genfstab -L /mnt >> /mnt/etc/fstab     # Labels instead of UUIDs
```

Copy any other configuration files to the new system in `/mnt` (such as netctl profiles in `/etc/netctl`)  
Then chroot to it:
```bash
arch-chroot /mnt /bin/bash
nano /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8, as well as other needed localizations
locale-gen
```

Create `/etc/locale.conf`, where `LANG` refers to the first column of an uncommented entry in `/etc/locale.gen`:
```bash
nano /etc/locale.conf
LANG=en_US.UTF-8

nano /etc/vconsole.conf
KEYMAP=pt-latin1
#FONT=Lat2-Terminus16
```

#### Update time
```bash
tzselect
# ln -s /usr/share/zoneinfo/Zone/SubZone /etc/localtime
ln -s /usr/share/zoneinfo/Europe/Lisbon /etc/localtime
hwclock --systohc --utc
```

### Install GRUB

#### MBR:
```bash
pacman -S grub os-prober
grub-install --target=i386-pc /dev/sdx
grub-mkconfig -o /boot/grub/grub.cfg
```

#### OSX EFI:
See bottom notes.

### Network

Add your chosen computer name in `ano /etc/hostname` and after each localhost entry in `nano /etc/hosts`

```bash
pacman -S iw wpa_supplicant dialog
```
Reconfigure Wireless:
`wifi-menu`

Set the root password:
```bash
passwd
```

Reboot:
```bash
exit
umount -R /mnt
reboot
```

## Post-install

Enable multilib repository:  
Uncomment from /etc/pacman.conf:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

```bash
pacman -Sy
```

### Install Graphical Environment

```bash
pacman -S xorg-server xorg-server-utils
pacman -S xf86-video-intel mesa-libgl lib32-mesa-libgl  # Intel drivers
pacman -S xf86-input-synaptics                          # If asked, choose evdev.

pacman -S gnome gnome-extra

systemctl enable gdm.service
systemctl enable NetworkManager.service
```

### Configure Synaptics touchpad:
```bash
nano /usr/share/X11/xorg.conf.d/70-synaptics.conf
# Add to touchpad catchall
        Option "VertScrollDelta" "-222"
        Option "HorizScrollDelta" "-222"
        Option "TapButton1" "1"
        Option "MaxTapTime" "75"
        MatchDevicePath "/dev/input/event*"
```

### Add another user to SUDOers
```bash
pacman -S sudo
EDITOR="nano" visudo

# Add:
<USERNAME>        ALL=(ALL) ALL

# Uncomment the line:
Defaults env_keep += "HOME"
```

### Printing:
```bash
sudo pacman -S cups ghostscript gsfonts cups-pdf hplip system-config-printer
sudo systemctl enable org.cups.cupsd.service
sudo systemctl start org.cups.cupsd.service
```

### Useful packages:
AUR: `ttf-ms-fonts adobe-source-han-sans-otc-fonts`

### Android MTP:
```bash
sudo pacman -S libmtp gvfs-mtp gvfs-gphoto2
sudo reboot
```

## Macbook Air

### GRUB

#### GRUB-EFI (EFI, OSX):
```bash
pacman -S grub-efi-x86_64
nano /etc/default/grub
GRUB_TIMEOUT=0
# Uncomment GRUB_HIDDEN_TIMEOUT and ..._QUIET
GRUB_CMDLINE_LINUX_DEFAULT="quiet acpi_osi="

grub-mkconfig -o /boot/grub/grub.cfg
grub-mkstandalone -o boot.efi -d /usr/lib/grub/x86_64-efi -O x86_64-efi --compress=xz /boot/grub/grub.cfg
mkdir /mnt/usbdisk && mount /dev/sdy /mnt/usbdisk
cp boot.efi /mnt/usbdisk/
```

Reboot to OSX, Erase the bootloader partition
```bash
cd /Volumes/Linux\ Bootloader
mkdir System mach_kernel
cd System
mkdir -p Library/CoreServices
cd Library/CoreServices
touch SystemVersion.plist
cp /Volumes/usbdrive/boot.efi .
```

Edit SystemVersion.plist to look like this
```xml
<xml version="1.0" encoding="utf-8"?>
<plist version="1.0">
<dict>
    <key>ProductBuildVersion</key>
    <string></string>
    <key>ProductName</key>
    <string>Linux</string>
    <key>ProductVersion</key>
    <string>Arch Linux</string>
</dict>
</plist>
```

To make Arch the default (and avoid pressing Alt/Option at boot):  
Enable bless in El Capitan:  
  1. Boot to Recovery OS by restarting your machine and holding down the Command and R keys at startup. (Or option and choosing "Recovery 10.xx")
  2. Launch Terminal from the Utilities menu.
  3. Enter the following command: csrutil enable
  4. Reboot

```bash
sudo bless --device /dev/disk0s4 --setBoot
```


### Disable Bluetooth
```bash
sudo nano /etc/modprobe.d/50-disabling.conf
blacklist bluetooth
blacklist btusb
```


## VirtualBox
Install Guest Additions:
```bash
pacman -S virtualbox-guest-utils      # (use arch)
systemctl enable vboxservice.service
```

## VMWare - Virtual Machine

Possible Key: `GA1T0-AXGDL-484GZ-4YWNX-PV0V8`

https://wiki.archlinux.org/index.php/VMware/Installing_Arch_as_a_guest

### Open VM Tools (open source)
```bash
sudo pacman -S open-vm-tools
cat /proc/version > /etc/arch-release
# (shared folders: AUR open-vm-tools-dkms)
sudo systemctl enable vmware-vmblock-fuse.service
# (shared folders: sudo systemctl enable dkms.service)
```

### VMWare Tools (official, closed source)
Download from http://softwareupdate.vmware.com/cds/vmw-desktop/fusion/

```bash
for x in {0..6}; do mkdir -p /etc/init.d/rc${x}.d; done
pacman -S net-tools gtkmm xf86-input-vmmouse xf86-video-vmware mesa
```

Extract and install with
```bash
./vmware-install.pl   # Choose /etc/init.d/ for the rc0.d...rc6.d directory
```
