# Arch Linux

## Basics

| Command                 | Action                               |
|-------------------------|--------------------------------------|
| `pacman -S <package>`   | Install package                      |
| `pacman -R <package>`   | Remove package                       |
| `pacman -Ss <query>`    | Search package                       |
| `pacman -Syu`           | Update packages                      |
| `pacman -Qo <file>`      | Check which package installed <file>  |

## Arch User Repository (AUR)

User-provided software, available in https://aur.archlinux.org/index.php

### Install packages manually

1. Download the tarball  
  `curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/package_name.tar.gz`
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

### Install packages automatically with cower (AUR)
```bash
cd aur
cower -s <name>           # search
cower -d <name>           # download
cd <name>
makepkg -sri
```

#### Update packages automatically
`cower -duf --timeout=20`

## Installation guide (2018-02-03)

### First steps

Burn the ISO to a USB
```bash
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync
```

List available keyboard layouts
```bash
ls /usr/share/kbd/keymaps/**/*.map.gz
```

Change keyboard layout
```bash
loadkeys pt-latin1
```

Check if wired connection is available
```bash
ping archlinux.org
```

If not, enable wired connection
```bash
ip link                                       # to find out <interface>
systemctl enable dhcpcd@<interface>.service
dhcpcd
```

Configure Wireless  
`wifi-menu`

Check if UEFI mode is on
```bash
ls /sys/firmware/efi/efivars      # only exists if UEFI
```

Start time sync and check status
```bash
timedatectl status
timedatectl set-ntp true
timedatectl status
```

### Setup disk

Check disks
```bash
lsblk                   # or: fdisk -l
fdisk -l /dev/sdx       # where sdx is your desired disk
```

Check current partition table  
`parted /dev/sdx print`

#### Swap  Partition

##### Partitioning using MBR, / + swap
**Note**: This bricked an Acer's HDD, maybe because Swap comes first or because lack of formatting
```bash
parted /dev/sdx
mklabel msdos                             # create new partition table
mkpart primary linux-swap 1MiB 6GiB
mkpart primary ext4 6GiB 100%
set 2 boot on                             # if dual-booting, don't change boot flag
quit
```

##### Partitioning using MBR, / without swap
```bash
parted /dev/sdx
mklabel msdos                             # create new partition table
mkpart primary ext4 1MiB 100%
set 1 boot on                             # if dual-booting, don't change boot flag
quit
```

Check partitions  
```bash
lsblk /dev/sdx
```

Format ext4 partitions  
```bash
mkfs.ext4 /dev/sdxY
```

Activate swap partition if any
```bash
mkswap /dev/sdxY
swapon /dev/sdxY
```

Mount root partition  
```bash
mount /dev/sdxY /mnt
```

Configure swap file if not using a partition
```bash
fallocate -l 4G /mnt/swapfile
chmod 600 /mnt/swapfile
mkswap /mnt/swapfile
swapon /mnt/swapfile
```

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
genfstab -U /mnt >> /mnt/etc/fstab
# genfstab -L /mnt >> /mnt/etc/fstab     # Alternative: Labels instead of UUIDs
```

Check the generated fstab and fix it:
```bash
nano /mnt/etc/fstab
```
- Remove `/mnt`from swapfile if any
- Add option `discard` to SSD drives

Copy any other configuration files to the new system in `/mnt` (such as netctl profiles in `/etc/netctl`)  
Then chroot to it:
```bash
arch-chroot /mnt
```

#### Set time zone
```bash
tzselect
# ln -sf /usr/share/zoneinfo/Zone/SubZone /etc/localtime
ln -sf /usr/share/zoneinfo/Europe/Lisbon /etc/localtime
hwclock --systohc
```

If using Windows, remember to change it to UTC hardware clock as well:
```dos
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
```
It may also be necessary to disable internet time update in Windows (at least timezone update).  
Alternatively, you can use Arch in localtime as well (not recommended):
```bash
timedatectl set-local-rtc 1
```

#### Configure locale

```bash
nano /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8, as well as other needed localizations
locale-gen
```

Create `/etc/locale.conf`, where `LANG` refers to the first column of an uncommented entry in `/etc/locale.gen`:
```bash
nano /etc/locale.conf
LANG=en_US.UTF-8
```

Setup console keymap and font (if needed):
```bash
nano /etc/vconsole.conf
KEYMAP=pt-latin1
#FONT=Lat2-Terminus16
```

### Hostname

Add your chosen computer name in `nano /etc/hostname`. Make a matching edit to `/etc/hosts`:

```bash
127.0.0.1 localhost
::1		localhost
127.0.1.1	<myhostname>.localdomain	<myhostname>
```

```bash
pacman -S iw wpa_supplicant dialog
```

Reconfigure Wireless:
```bash
wifi-menu
```

### Root password

Set the root password:
```bash
passwd
```


### Bootloader: GRUB

#### MBR
```bash
pacman -S grub os-prober
grub-install --target=i386-pc /dev/sdx
grub-mkconfig -o /boot/grub/grub.cfg
```

#### OSX EFI
See bottom notes.


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
pacman -S xorg-server

# Graphics drivers:
pacman -S xf86-video-intel mesa lib32-mesa         # Intel drivers
pacman -S nvidia nvidia-libgl lib32-nvidia-libgl   # Nvidia drivers

pacman -S gnome gnome-extra

systemctl enable gdm.service
systemctl enable NetworkManager.service
```

To check (later) that Direct Rendering is being used:  
```bash
sudo pacman -S mesa-demos
glxinfo | grep direct
```

### Add another user
```bash
useradd -m -G wheel -s /bin/bash <username>      # -m: creates home dir, wheel is the administrators group
passwd <username>                                # setup password
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

### Printing
```bash
sudo pacman -S cups ghostscript gsfonts cups-pdf hplip system-config-printer
sudo systemctl enable org.cups.cupsd.service
sudo systemctl start org.cups.cupsd.service
```

## Configure GNOME

### Additional packages

```bash
pacman -S gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly gst-libav   # GStreamer codecs
pacman -S gnome-tweak-tool arc-gtk-theme arc-icon-theme   # Appearance
pacman -S atom openssh  # Development
```

GNOME extensions (visit the links in the GNOME Web Browser):  
- https://extensions.gnome.org/extension/307/dash-to-dock/


AUR packages:
```bash
cower
google-chrome
```

AUR package candidates to test:
```bash
ttf-ms-fonts
adobe-source-han-sans-otc-fonts
```

### Settings 

- Region & Language > Input Sources > Your desired keyboard
- Details > Date & Time > Automatic Date & Time / Time Zone
- Details > Users > Photo


### Gnome tweak tool
- Appearance > Themes > Applications > Arc-Dark
- Appearance > Themes > Icons > Arc
- Desktop > Icons on Desktop
- Top Bar
- Windows > Titlebar Buttons

### Dash-to-dock (right click on dock's Applications icon)
- Position and size > Icon size: 32
- Launchers > Move the applications button at the beginning of the dock
- Appearance > Shrink the dash
- Appearance > Show windows counter indicators
- Appearance > Customize opacity (70%)

### Templates
```bash
touch ~/Templates/Text\ Document.txt
```

#### Alt-tab switch only between workspace apps
```bash
gsettings set org.gnome.shell.app-switcher current-workspace-only true
```

### Android Studio
(package in AUR)  
To enable creation of SD Cards (and therefore AVDs):
```bash
sudo pacman -S lib32-gcc-libs lib32-ncurses
echo "export ANDROID_HOME=/opt/android-sdk" >> .bashrc
```

### Android MTP
```bash
sudo pacman -S libmtp gvfs-mtp gvfs-gphoto2
sudo reboot
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
