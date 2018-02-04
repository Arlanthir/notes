# Arch Linux

## Basics

| Command                 | Action                               |
|-------------------------|--------------------------------------|
| `pacman -S <package>`   | Install package                      |
| `pacman -R <package>`   | Remove package                       |
| `pacman -Ss <query>`    | Search package                       |
| `pacman -Syu`           | Update packages                      |
| `pacman -Qo <file>`     | Check which package installed <file> |

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
`lsblk                           # or: fdisk -l`

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
`lsblk /dev/sdx`

Format ext4 partitions  
`mkfs.ext4 /dev/sdxY`

Activate swap partition if any
```bash
mkswap /dev/sdxY
swapon /dev/sdxY
```

Mount root partition  
`mount /dev/sdxY /mnt`

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
# genfstab -U /mnt >> /mnt/etc/fstab
genfstab -L /mnt >> /mnt/etc/fstab     # Labels instead of UUIDs
```

Check the generated fstab (in particular remove `/mnt` from swapfile if needed):
```bash
cat /mnt/etc/fstab
```

Copy any other configuration files to the new system in `/mnt` (such as netctl profiles in `/etc/netctl`)  
Then chroot to it:
```bash
arch-chroot /mnt
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

If using Windows, remember to change it to UTC hardware clock as well:
```dos
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
```
It may also be necessary to disable internet time update in Windows (at least timezone update).  
Alternatively, you can use Arch in localtime as well (not recommended):
```bash
timedatectl set-local-rtc 1
```

### Install GRUB

#### MBR
```bash
pacman -S grub os-prober
grub-install --target=i386-pc /dev/sdx
grub-mkconfig -o /boot/grub/grub.cfg
```

#### OSX EFI
See bottom notes.

### Network

Add your chosen computer name in `nano /etc/hostname` and after each localhost entry in `nano /etc/hosts`

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
# Important optional packages
pacman -S xf86-video-intel mesa-libgl lib32-mesa-libgl  # Intel drivers
pacman -S nvidia nvidia-libgl lib32-nvidia-libgl        # Nvidia drivers
pacman -S xf86-input-synaptics                          # If asked, choose evdev.

pacman -S gnome gnome-extra

systemctl enable gdm.service
systemctl enable NetworkManager.service
```

To check (later) that Direct Rendering is being used:  
```bash
sudo pacman -S mesa-demos
glxinfo | grep direct
```

### Configure Synaptics touchpad
```bash
nano /usr/share/X11/xorg.conf.d/70-synaptics.conf
# Add to touchpad catchall
        Option "VertScrollDelta" "-222"
        Option "HorizScrollDelta" "-222"
        Option "TapButton1" "1"
        Option "MaxTapTime" "75"
        MatchDevicePath "/dev/input/event*"
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

### Useful packages
Pacman: `atom openssh gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly gst-libav arc-gtk-theme arc-icon-theme`  
AUR: `cower` + `google-chrome`  
AUR Candidates (need test): `ttf-ms-fonts adobe-source-han-sans-otc-fonts`

### GNOME Extensions and tweaks
Visit URLs in the GNOME Web browser

- https://extensions.gnome.org/extension/307/dash-to-dock/

#### Mouse scroll wheel speed (Introduces more problems than it fixes)
Install AUR package `imwheel`

`nano .imwheelrc`:

```
".*"
None,      Up,   Button4, 3
None,      Down, Button5, 3
Control_L, Up,   Control_L|Button4
Control_L, Down, Control_L|Button5
Shift_L,   Up,   Shift_L|Button4
Shift_L,   Down, Shift_L|Button5
```

Add to startup:

`nano ~/.config/autostart/imwheel.desktop`:

[Desktop Entry]
Type=Application
Exec=imwheel --kill --buttons 45
Hidden=true
X-GNOME-Autostart-enabled=true
Name=imwheel
Comment=Increase mouse scroll wheel speed


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

## Macbook Air

### GRUB

#### GRUB-EFI (EFI, OSX)
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

### Macbook Air Tweaks

#### Suggested AUR packages
- broadcom-wl-dkms
- bcwc-pcie-firmware
- bcwc-pcie-dkms

#### Fix suspend backlight in Macbook:

Install AUR package `mba6x_bl-dkms-git`
```bash
# OLD:
# copy to /etc/systemd/system/backlightfix.service (at least on Arch Linux)
# sudo systemctl enable backlightfix.service

[Unit]
Description=Remove and reload mba6x to workaround no brightness bug


[Service]
ExecStart=/bin/sh -c "modprobe -r mba6x_bl && modprobe mba6x_bl"
Type=oneshot


[Install]
WantedBy=multi-user.target
```

#### Check CPU specs
```bash
cat /proc/cpuinfo
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

#### Check CPU freq
```bash
cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_cur_freq
```

#### kworker issue
Sometime with the addition of Yosemite, some users found that kworker CPU usage will spike, as discussed here. This is sometimes the result of runaway ACPI interrupts.

To check and see, you can count the number of recent ACPI interrupts and see if any of them are out of control.
```bash
grep . -r /sys/firmware/acpi/interrupts/
```
If you see that one particular interrupt is out of control (possibly GPE66 or GPE4E), i.e., registering hundreds of thousands of lines, you can try disabling it (replace XX with the runaway interrupt):
```bash
echo "disable" > sudo tee /sys/firmware/acpi/interrupts/gpeXX
```
To make permanent:
```bash
sudo nano /etc/systemd/system/suppress-gpe4E.service

[Unit]
Description=Disables GPE4E, an interrupt that is going crazy on Macs
[Service]
ExecStart=/usr/bin/bash -c 'echo "disable" > /sys/firmware/acpi/interrupts/gpe4E'
[Install]
WantedBy=multi-user.target

sudo systemctl enable suppress-gpe4E.service
sudo systemctl start suppress-gpe4E.service
```

#### Making the fans work
```bash
# Add to /etc/modules:
applesmc
coretemp
# Install the AUR package mbpfan-git, controls the fan speed according to the current temperature
sudo systemctl enable mbpfan.service

# Check that /sys/devices/platform/applesmc.768/fan1_manual is set to 0 (at least after reboot)

Install AUR package thermald
sudo systemctl enable thermald.service

sudo pacman -S cpupower
sudo systemctl enable cpupower
sudo systemctl start cpupower
sudo cpupower frequency-set -g powersave
```

#### Monitor power usage
```bash
sudo pacman -S tlp acpi_call powertop
sudo systemctl enable tlp
sudy systemctl start tlp
sudo powertop --calibrate # wait a long time
sudo powertop # leave open for a long time
sudo powertop --auto-tune

# When it works, make it permanent:

sudo nano /etc/systemd/system/powertop.service
[Unit]
Description=Powertop Service
[Service]
Type=oneshot
ExecStart=/usr/bin/powertop --auto-tune
[Install]
WantedBy=multi-user.target

sudo systemctl enable powertop
sudo systemctl start powertop
```

#### Try setting a max CPU freq
```bash
sudo nano /etc/default/cpupower
governor='powersave'
max_freq="2.6GHz"
```

Monitor temps:
```bash
sudo pacman -S psensor
```

#### Fix wakeups

Check wakeup reasons:
```bash
journalctl | grep -i wake
```

Disable problematic wakeups;
```bash
cat /proc/acpi/wakeup
echo XHC1 > sudo tee /proc/acpi/wakeup
# to make permanent:
sudo nano /etc/udev/rules.d/90-xhc_sleep.rules
# disable wake from S3 on XHC1
SUBSYSTEM=="pci", KERNEL=="0000:00:14.0", ATTR{power/wakeup}="disabled"
```

#### Set FN keys to F1-F12 instead of brightness, etc
```bash
echo 2 > sudo tee /sys/module/hid_apple/parameters/fnmode
```

Reverse:
```bash
echo 1 > sudo tee /sys/module/hid_apple/parameters/fnmode
```

#### Enable 3-finger gestures

##### xSwipe
```bash
sudo pacman -R xf86-input-synaptics
# install AUR xf86-input-synaptics-xswipe-git perl-x11-guitest perl-smart-comments
git clone https://github.com/iberianpig/xSwipe.git
cd xSwipe/
nano eventKey.cfg                                    # or nScroll/eventKey.cfg if natural scroll is wanted
# configure it, in gnome there's a DOW instead of DOWN, etc

sudo nano /etc/X11/xorg.conf.d/50-synaptics.conf

        Option "VertScrollDelta" "-222"
        Option "HorizScrollDelta" "-222"
        Option "TapButton1" "1"
        Option "MaxTapTime" "80"

        Option "Protocol" "event"
        Option "SHMConfig" "on"

sudo reboot
```

To test:
```bash
perl xSwipe.pl -d 0.25 -m 30
```

Add to autostart:
```bash
nano ~/.config/autostart/xswipe.desktop

[Desktop Entry]
Type=Application
Path=/home/arlanthir/xSwipe/
Exec=perl xSwipe.pl -n -d 0.25 -m 30
Hidden=false
X-GNOME-Autostart-enabled=true
Name=xSwipe
Comment=Listen to touchpad gestures
```

##### TODO alternative
Test https://aur.archlinux.org/packages/xf86-input-mtrack-git/


#### More tips in
https://medium.com/@philpl/arch-linux-running-on-my-macbook-2ea525ebefe3#.h3deucjeu
https://mchladek.me/post/arch-mbp/






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
