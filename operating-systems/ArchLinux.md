# Arch Linux

## Basics

| Command                 | Action                                     |
|-------------------------|--------------------------------------------|
| `pacman -S <package>`   | Install package                            |
| `pacman -R <package>`   | Remove package                             |
| `pacman -Ss <query>`    | Search package                             |
| `pacman -Syu`           | Update packages                            |
| `pacman -Sc`            | Clear cache                                |
| `pacman -Qo <file>`     | Check which package installed &lt;file&gt; |

## Arch User Repository (AUR)

User-provided software, available in https://aur.archlinux.org/index.php

### Install packages manually

1. Download the tarball  
  `curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/package_name.tar.gz`
2. Extract it, go to the folder  
  `tar -xvf package_name.tar.gz -C dirname`  
  `cd dirname`
3. Create package and install it  
  `makepkg -sri` (install dependencies, remove dependencies after build, install after build)

If makepkg complains about PGP keys (ex: `1EB2638FF56C0C53`):  
`gpg --recv-keys --keyserver hkp://pgp.mit.edu 1EB2638FF56C0C53`

To install using root, use su (not recommended):  
`su -s /bin/bash nobody`

### List AUR packages
`pacman -Qm`

### Install packages automatically with yay (AUR)
```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -sri
```

`yay` supports the same operations as `pacman`  
To only download the PKGBUILD of a package, do:

`yay -G <package_name>`


## Installation guide (2019-10-27)

### First steps

Burn the ISO to a USB
```bash
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync
# If EFI boot hangs, repeat the `dd` command a couple of times (this was actually in the wiki...)
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

##### Partitioning using GPT, / without swap
```bash
parted /dev/sdx
mklabel gpt
mkpart primary fat32 1MiB 261MiB
set 1 esp on
mkpart primary ext4 261MiB 100%
quit
mkfs.fat -F32 /dev/sdx1
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
pacstrap -i /mnt base linux linux-firmware base-devel nano dhcpcd dosfstools e2fsprogs ntfs-3g
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

Add optional NTFS drive (remember to install ntfs-3g and to create the mnt folder):
```
#UUID=...    /mnt/data   ntfs-3g   rw,exec,permissions,auto,x-gvfs-show   0 0
UUID=...    /mnt/data   ntfs-3g   defaults,uid=1000,gid=1000,exec,x-gvfs-show   0 0
defaults,uid=1000,gid=1000,dmask=000,fmask=000,exec,x-gvfs-show
```

If mounting ntfs-3g drives with write permissions, make sure Windows fast startup is disabled, so Windows fully unmount disks.

Copy any other configuration files to the new system in `/mnt` (such as netctl profiles in `/etc/netctl`)  
Then chroot to it:
```bash
arch-chroot /mnt
```

#### Set time zone
```bash
tzselect
# ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
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

#### EFI
```bash
pacman -S grub efibootmgr os-prober
# If intel:
pacman -S intel-ucode
# If amd:
pacman -S amd-ucode
mkdir /efi
mount /dev/sdx1 /efi
grub-install --target=x86_64-efi --efi-directory=efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

#### MBR
```bash
pacman -S grub os-prober
grub-install --target=i386-pc /dev/sdx
grub-mkconfig -o /boot/grub/grub.cfg
```

Enable dhcpcd if needed:
```bash
ip link                                       # to find out <interface>
systemctl enable dhcpcd@<interface>.service
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

### Add another user
```bash
useradd -m -G wheel -s /bin/bash <username>      # -m: creates home dir, wheel is the administrators group
passwd <username>                                # setup password
```

### Add another user to SUDOers
```bash
pacman -S sudo
EDITOR="nano" visudo

# OLD: # Add:
# OLD: <USERNAME>        ALL=(ALL) ALL

# Instead, uncomment the line:
%wheel ALL=(ALL) ALL

# Uncomment the line:
Defaults env_keep += "HOME"
```

### Install Graphical Environment

```bash
pacman -S xorg-server

# Graphics drivers:
pacman -S xf86-video-intel mesa lib32-mesa         # Intel drivers
pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings   # Nvidia drivers
pacman -S vulkan-icd-loader lib32-vulkan-icd-loader # Vulkan support
```

To check (later) that Direct Rendering is being used:  
```bash
sudo pacman -S mesa-demos
glxinfo | grep direct
```

#### GNOME

```bash
pacman -S gnome gnome-extra chrome-gnome-shell

systemctl enable gdm.service
systemctl enable NetworkManager.service
```

When using nvidia proprietary drivers, it may be needed to uncomment the line `#WaylandEnable=false` in `/etc/gdm/custom.conf`.

##### Printing

```bash
sudo pacman -S cups cups-pdf ghostscript gsfonts hplip system-config-printer
sudo systemctl enable org.cups.cupsd.service
sudo systemctl start org.cups.cupsd.service
```

##### Additional packages

```bash
pacman -S gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly gst-libav   # GStreamer codecs
pacman -S gnome-tweak-tool arc-gtk-theme arc-icon-theme   # Appearance
pacman -S atom openssh  # Development
pacman -S ttf-liberation adobe-source-han-sans-otc-fonts adobe-source-han-serif-otc-fonts # Fonts
```

AUR packages:
```bash
gnome-shell-extension-dash-to-dock
```

Other GNOME extensions (visit the links in the GNOME Web Browser):  
- https://extensions.gnome.org/extension/307/dash-to-dock/


##### Settings 

###### Settings application
- Region & Language > Input Sources > Your desired keyboard
- Details > Date & Time > Automatic Date & Time / Time Zone
- Details > Users > Photo


###### Gnome tweak tool
- Appearance > Themes > Applications > Arc-Dark
- Appearance > Themes > Icons > Arc
- Desktop > Icons on Desktop
- Top Bar
- Windows > Titlebar Buttons

###### Dash-to-dock (right click on dock's Applications icon)
- Position and size > Icon size: 32
- Launchers > Move the applications button at the beginning of the dock
- Appearance > Shrink the dash
- Appearance > Show windows counter indicators
- Appearance > Customize opacity (70%)

###### Other applications
- atom: config.cson and keymap.cson
- terminal: Profile Preferences > Colors > White on Black

###### Templates
```bash
touch ~/Templates/Text\ Document.txt
```

###### Alt-tab switch only between workspace apps
```bash
gsettings set org.gnome.shell.app-switcher current-workspace-only true
```


#### KDE

```bash
pacman -S plasma-meta kde-applications-meta sddm sddm-kcm
# Choose phonon-qt5-vlc and cronie if asked
systemctl enable sddm
localectl set-x11-keymap pt
```

##### Printing

```bash
sudo pacman -S cups cups-pdf print-manager hplip system-config-printer
sudo systemctl enable org.cups.cupsd.service
```

##### Additional Packages

###### Pacman
```bash
latte-dock
materia-kde kvantum-theme-materia
kvantum-qt5
papirus-icon-theme
```

###### AUR
```bash
breeze-blurred-git
```

##### Settings

###### System Settings
- Workspace Behavior > Desktop Effects > Blur > Blur > 10
- Workspace Behavior > Desktop Effects > Blur > Noise > 6
- Workspace Behavior > Desktop Effects > Translucency > Menus > 8
- Workspace Behavior > General Behavior > Double-click
- Workspace Behavior > Screen Locking > Automatically (off)
- Workspace Behavior > Virtual Desktops > Configure as desired (1 Row or 2 Rows)
- Workspace Behavior > Virtual Desktops > Navigation wraps around (off)
- Window Management > Kwin Scripts > MinimizeAll (on)
- Shortcuts > Global Shortcuts > KWin > Switch One Desktop Down > Ctrl + Alt + Down
- Shortcuts > Global Shortcuts > KWin > Switch One Desktop to the Left > Ctrl + Alt + Left
- Shortcuts > Global Shortcuts > KWin > Switch One Desktop to the Right > Ctrl + Alt + Right
- Shortcuts > Global Shortcuts > KWin > Switch One Desktop Up > Ctrl + Alt + Up
- Shortcuts > Global Shortcuts > KWin > Window One Desktop Down > Ctrl + Alt + Shift+ Down
- Shortcuts > Global Shortcuts > KWin > Window One Desktop to the Left > Ctrl + Alt + Shift+ Left
- Shortcuts > Global Shortcuts > KWin > Window One Desktop to the Right > Ctrl + Alt + Shift+ Right
- Shortcuts > Global Shortcuts > KWin > Window One Desktop Up > Ctrl + Alt + Shift+ Up
- Personalization > Account Details > User Manager > Photo
- Personalization > Notifications > Do not disturb when screens are mirrored (off)
- Personalization > Regional Settings > Formats
- Hardware > Input Devices > Layouts
- Hardware > Power Management > Energy Saving > Screen Energy Saving (60 min)
- Hardware > Printers

###### System tray

Right click > Configure System Tray

General > Extra Items:
- Clipbard (off)
- Instant Messaging (off)
- Vaults (off)

Entries > Volume Control > Hidden

###### Calendar Widgets

1. Configure the Google Calendar account in KOrganizer
2. Add the digital clock widget and configure it using the PIM plugin
3. The events should now appear in the Calendar widget as well

###### Chrome
- Right click on title bar > Use system title bar and borders (off)

###### Konsole

- Settings > Edit Current Profile > Appearance > Edit > Blur background (on)
- Settings > Edit Current Profile > Appearance > Edit > Background transparency (25%)

###### Kvantum Manager

Themes:

https://github.com/Akava-Design/Akava-Kv

- Kvantum Manager > Install/Update Theme > Select a Kvantum theme folder > Install this theme
- Kvantum Manager > Change/Delete Theme > Akava-Kv > Use this theme
- Kvantum Manager > Configure Active Theme > Hacks > Transparent Dolphin view

- System Settings > Application Style > BreezeBlurred > Configure > Draw titlebar background gradient (off)
- System Settings > Application Style > BreezeBlurred > Configure > Draw separator between titlebar and window (off)
- System Settings > Application Style > BreezeBlurred > Configure > Opacity (40%)

- System Settings > Workspace Behavior > Desktop Effects > Blur > Blur > 15
- System Settings > Workspace Behavior > Desktop Effects > Blur > Noise > 7

https://github.com/Akava-Design/Akava-Theme

`cp -r Akava ~/.local/share/plasma/desktoptheme`

- System Settings > Appearance > Plasma Style > Akava

https://github.com/Akava-Design/Akava-Colors

`cp Akava.colors ~/.local/share/color-schemes/`

- System Settings > Appearance > Colors > Akava

https://github.com/Akava-Design/Akava-Konsole

`cp Akava.colorscheme ~/.local/share/konsole`

- Konsole > Settings > Edit Current Profile > Appearance > Color scheme & font > Akava


## Other programs

### Pacman packages
```bash
openssh
steam
```

### AUR packages
```bash
yay                 # (manual installation)
google-chrome
visual-studio-code-bin
```


### Syncthing

```bash
pacman -S syncthing
systemctl --user enable --now syncthing.service
```

Web interface is available at http://localhost:8384/

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

### Equalizer
```bash
pacman -S pulseaudio-equalizer pavucontrol # Equalizer
pactl load-module module-equalizer-sink
pactl load-module module-dbus-protocol
```

To persist:
```
sudo nano /etc/pulse/default.pa

Add:
### Load the integrated PulseAudio equalizer and D-Bus module
load-module module-equalizer-sink
load-module module-dbus-protocol
```

To run the GUI equalizer:
```
qpaeq
```

**Note**: If `qpaeq` has no effect, run `pavucontrol` and change "ALSA Playback on" to "FFT based equalizer on ..." while the media player is running.

## Troubleshooting

Log of gnome-shell extensions:

```bash
journalctl /usr/bin/gnome-shell -f -o cat
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
