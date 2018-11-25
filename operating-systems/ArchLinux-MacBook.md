# Arch Linux MacBook Air (Early 2014, MacBookAir6,2)

## Installation guide (2018-02-03)

### Requirements
- Already prepared Arch Linux live USB
- Android phone with USB tethering

### First steps

For some reason, the mac-pt-latin1 keyboard layout is not working, do:
```bash
loadkeys pt-latin1
```

Connect the USB tethering Android phone and check for internet connection:
```bash
ping archlinux.org
```

If down, enable wired connection
```bash
ip link                                        # to find out <interface>
# systemctl enable dhcpcd@<interface>.service  # for permanent <interfaces>, not USB tethering
dhcpcd <interface>
```
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
lsblk                    # or: fdisk -l
fdisk -l /dev/sdx        # where sdx is your desired disk
```

##### Partitioning using EFI, / without swap (Macbook)

**Note**: This partitioning + bootloader scheme seems outdated (yet it seems to work). Check wiki for newer alternatives.

Remember to allocate 128 MiB in an HFS+ (af00) partition for a Linux Boot Loader.
macOS likes to see a 128 MiB gap after partitions, so when you create the first partition after the last macOS partition, type in +128M when cgdisk asks for the first sector for the partition.

```bash
cgdisk /dev/sdx
128 MiB           Linux boot loader
Remaining Space   Linux filesystem    
Type codes: Ext4 is 8300 (the default), Linux swap is 8200
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
genfstab -U /mnt >> /mnt/etc/fstab
# genfstab -L /mnt >> /mnt/etc/fstab     # Alternative: Labels instead of UUIDs
```

Check the generated fstab and fix it:
```bash
nano /mnt/etc/fstab
```

- Remove `/mnt` from swapfile if any
- Add option discard to SSD drives


Chroot to the new system:
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

#### Hostname

Add your chosen computer name in `nano /etc/hostname`. Make a matching edit to `nano /etc/hosts`:
```bash
127.0.0.1	localhost
::1		localhost
127.0.1.1	<myhostname>.localdomain	<myhostname>
```

#### Root password

Set the root password:
```bash
passwd
```

#### Bootloader: GRUB-EFI (EFI, OSX)
```bash
pacman -S grub-efi-x86_64
nano /etc/default/grub
GRUB_TIMEOUT=0
# Uncomment GRUB_HIDDEN_TIMEOUT and ..._QUIET
GRUB_CMDLINE_LINUX_DEFAULT="quiet acpi_osi=Darwin"

grub-mkconfig -o /boot/grub/grub.cfg
grub-mkstandalone -o boot.efi -d /usr/lib/grub/x86_64-efi -O x86_64-efi --compress=xz /boot/grub/grub.cfg
mkdir /mnt/usbdisk 
fdisk -l # to check which /dev/sdyz is the USB disk
mount /dev/sdyz /mnt/usbdisk
cp boot.efi /mnt/usbdisk/
```

Reboot to OSX, Erase the bootloader partition:
```bash
exit
umount -R /mnt
reboot
```

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
pacman -S xf86-video-intel mesa lib32-mesa  # Intel drivers
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

# Uncomment the line:
Defaults env_keep += "HOME"

# Add:
<USERNAME>        ALL=(ALL) ALL
```

### Touchpad
Install from AUR: `xf86-input-mtrack-git`

### Wifi
```bash
sudo pacman -S broadcom-wl-dkms linux-headers iw
```

### Printing
```bash
sudo pacman -S cups ghostscript gsfonts cups-pdf hplip system-config-printer
sudo systemctl enable org.cups.cupsd.service
sudo systemctl start org.cups.cupsd.service
```

### Disable Bluetooth
```bash
sudo nano /etc/modprobe.d/50-disabling.conf
blacklist bluetooth
blacklist btusb
```

#### Suggested AUR packages
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
TLP (do a new research to see if it's still recommended):
```bash
sudo pacman -S tlp acpi_call
sudo systemctl enable tlp
sudy systemctl start tlp
sudo systemctl enable tlp-sleep
sudy systemctl start tlp-sleep
```

Powertop:
```bash
sudo pacman -S powertop
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

#### More tips in
- https://wiki.archlinux.org/index.php/mac
- https://wiki.archlinux.org/index.php/MacBookPro11,x
- https://medium.com/@philpl/arch-linux-running-on-my-macbook-2ea525ebefe3#.h3deucjeu
- https://mchladek.me/post/arch-mbp/



