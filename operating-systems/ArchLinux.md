# Arch Linux

## Package Basics

| Command                   | Action                                                                 |
| ------------------------- | ---------------------------------------------------------------------- |
| `pacman -S <package>`     | Install package                                                        |
| `pacman -R <package>`     | Remove package                                                         |
| `pacman -Ss <query>`      | Search package                                                         |
| `pacman -Syu`             | Update packages                                                        |
| `pacman -Syudd`           | Update packages, skip dependency checks                                |
| `pacman -Syu --noconfirm` | Update packages without asking for confirmations (replacements, etc)   |
| `pacman -Sc`              | Clear cache                                                            |
| `pacman -Qi <package>`    | Show info (e.g. dependencies of &lt;package&gt; and what installed it) |
| `pacman -Ql <package>`    | List all files installed by &lt;package&gt;                            |
| `pacman -Qo <file>`       | Check which package installed &lt;file&gt;                             |
| `pacman -Qm`              | List installed packages from AUR                                       |
| `pacman -Sg <group>`      | Check which packages belong to a group                                 |
| `sudo pacman -Qtdq \| sudo pacman -R -` | Remove all unused (orphan) packages                      |

*Note*: When your system is very outdated and updating it results in corrupted packages error, update the keyring first: `sudo pacman -S archlinux-keyring`.

## Arch User Repository (AUR)

User-provided software, available in https://aur.archlinux.org/index.php

### Install packages manually

```bash
# Download the tarball
curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/package_name.tar.gz
tar -xvf package_name.tar.gz -C dirname
cd dirname
makepkg -si # (install dependencies, install after build); or
makepkg -sri # (install dependencies, remove dependencies after build, install after build)
```

If makepkg complains about PGP keys (ex: `1EB2638FF56C0C53`):
```bash
gpg --recv-keys --keyserver hkp://pgp.mit.edu 1EB2638FF56C0C53
```

To install using root, use su (not recommended): `su -s /bin/bash nobody`


### Install packages automatically with paru (AUR)
```bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

`paru` supports the same operations as `pacman`

Additional commands:

| Command                   | Action                                                 |
| ------------------------- | ------------------------------------------------------ |
| `paru`                    | Upgrade all packages (including from official repos)   |
| `paru -Sua`               | Upgrade AUR packages only                              |
| `paru -G <package>`       | Download the PKGBUILD only                             |


### Install packages automatically with yay (AUR)

Previous alternative, written in Go. Equivalent commands to `paru` (and `pacman`).

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```


# Installation guide (2024-07-01)

Always compare to the [updated official guide](https://wiki.archlinux.org/title/installation_guide).

## First steps

Burn the ISO to a USB
```bash
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress
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

Confirm that wired connection is available
```bash
ping archlinux.org
```

Configure Wireless

```bash
iwctl
device list
# If needed:
# device <name> set-property Powered on
# adapter <adapter> set-property Powered on
station <name> scan
station <name> get-networks
station <name> connect <SSID>
exit
```

Confirm NTP is enabled
```bash
timedatectl
```

Otherwise
```bash
timedatectl status
timedatectl set-ntp true
timedatectl status
```

## Setup disk

Verify boot mode:
```bash
cat /sys/firmware/efi/fw_platform_size
# 64 bit UEFI (ideal), 32 bit UEFI (limited), non-existent (BIOS)
```

Check disks
```bash
fdisk -l | more         # or: lsblk
fdisk -l /dev/sdx       # where sdx is your desired disk; nvme should appear as nvme0n1
```

Check current partition table
```
parted /dev/sdx print
```

**Note**: Skip creating and formatting UEFI partition if it's already present.

### Partition

Partitioning using GPT, / without swap
```bash
parted /dev/sdx # Or /dev/nvme0n1
mklabel gpt
mkpart primary fat32 1MiB 261MiB
set 1 esp on
mkpart primary ext4 261MiB 100%
quit
```

<details>
  <summary>Previously: Partitioning using MBR (BIOS)</summary>

  Partitioning using MBR, / + swap  
  **Note**: This bricked an Acer's HDD, maybe because Swap comes first or because of lack of formatting
  ```bash
  parted /dev/sdx
  mklabel msdos                             # create new partition table
  mkpart primary linux-swap 1MiB 6GiB
  mkpart primary ext4 6GiB 100%
  set 2 boot on                             # if dual-booting, don't change boot flag
  quit
  ```

  Partitioning using MBR, / without swap
  ```bash
  parted /dev/sdx
  mklabel msdos                             # create new partition table
  mkpart primary ext4 1MiB 100%
  set 1 boot on                             # if dual-booting, don't change boot flag
  quit
  ```
</details><br>

Check partitions
```bash
lsblk /dev/sdx    # Or /dev/nvme0n1
```

Format UEFI partitions (only if newly created)
```bash
mkfs.fat -F32 /dev/sdx1   # Or /dev/nvme0n1pX
```

Format ext4 partitions  
```bash
mkfs.ext4 /dev/sdxY   # Or Or /dev/nvme0n1pY
```

Activate swap partition if any
```bash
mkswap /dev/sdxY
swapon /dev/sdxY
```

Mount root partition
```bash
mount /dev/sdxY /mnt    # Or /dev/nvme0n1p2
```
Mount EFI partition
```bash
# If using EFISTUB / systemd-boot
mount --mkdir /dev/sdx1 /mnt/boot   # Or /dev/nvme0n1p1
# If using GRUB
mount --mkdir /dev/sdx1 /mnt/efi    # Or /dev/nvme0n1p1
```

Configure swap file if not using a partition
```bash
mkswap -U clear --size 4G --file /mnt/swapfile
swapon /mnt/swapfile
```

## Installation

Update mirror list with preferred mirrors on the top:
```bash
nano /etc/pacman.d/mirrorlist
```
(To cut two lines, press `Ctrl+K` two times in a row)

Install:
```bash
pacstrap -K /mnt base linux linux-firmware base-devel git nano dosfstools e2fsprogs ntfs-3g # ALSO: intel-ucode or amd-ucode
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
- Add option `discard` to non-NVMe SSD drives

Add optional NTFS drive (remember to install ntfs-3g and to create the mnt folder):
```bash
lsblk -f                # print UUIDs

#UUID=...    /mnt/data   ntfs-3g   rw,exec,permissions,auto,x-gvfs-show   0 0
UUID=...    /mnt/data   ntfs-3g   defaults,uid=1000,gid=1000,dmask=022,fmask=133,exec,x-gvfs-show   0 0
# Consider mounting to /media/data instead of enabling x-gvfs-show
```

If mounting ntfs-3g drives with write permissions, make sure Windows fast startup is disabled, so Windows fully unmounts disks.

Chroot to the new system:
```bash
arch-chroot /mnt
```

### Set time zone
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

### Configure locale

```bash
nano /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8, as well as other needed localizations, such as pt_PT.UTF-8 UTF-8
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

### Network

```bash
pacman -S networkmanager
systemctl enable NetworkManager.service
nano /etc/hostname
<your_chosen_hostname>
```

### Root password

Set the root password:
```bash
passwd
```

## Bootloader

### UEFI systemd-boot

```bash
bootctl install
```

Configure manually:

/boot/loader/loader.conf:

```bash
default arch.conf
timeout 0 # Hides the menu, hold a key while booting to override
console-mode max
```

/boot/loader/entries/arch.conf

(remember to change disk/partition and intel to amd if applicable)

```bash
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img # or amd-ucode.img
initrd  /initramfs-linux.img
options root="/dev/nvme0n1p2" rw quiet loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3
```

To later check failed initializations (because of `quiet` option):
```bash
systemctl --failed
```

To update an existing installation:

```bash
bootctl update
```

To repair Windows boot if missing, refer to the Windows notes in this repository.

<details>
  <summary>Other bootloaders</summary>

  ### Bootloader: EFISTUB

  Boots directly into Linux kernel, useful if Linux is the only OS in the disk and only has one kernel.

  ```bash
  pacman -S efibootmgr
  # Troubleshooting: verify that /boot has initramfs-linux.img, intel-ucode.img/amd-ucode.img and vmlinuz-linux
  # They should automatically be populated when /boot is mounted and pacman installs kernels (linux) or *-ucode

  # Find the PARTUUID of the / partition:
  lsblk -o NAME,MOUNTPOINT,PARTUUID

  efibootmgr --disk /dev/nvme0n1 --part 1 --create --label "Arch Linux" --loader /vmlinuz-linux --unicode 'root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX rw initrd=\<intel or amd>-ucode.img initrd=\initramfs-linux.img' --verbose

  # To remove an entry inserted by mistake (e.g. Boot0000):
  efibootmgr -b 0000 -B

  # If no entry shows, it may be because the motherboard (e.g. MSI) expects a bootloader to be installed in the "fallback" location.
  # You can sort of workaround it by creating a dummy file in <boot partition>/EFI/BOOT/BOOTX64.EFI

  ```

  ### Bootloader: GRUB

  #### EFI
  ```bash
  pacman -S grub efibootmgr os-prober
  #mkdir /efi
  #mount /dev/sdx1 /efi
  grub-install --target=x86_64-efi --efi-directory=efi --bootloader-id=GRUB
  grub-mkconfig -o /boot/grub/grub.cfg

  # If GRUB doesn't run, it may be because the motherboard (e.g. MSI) expects it to be installed in the "fallback" location.
  # Use instead:
  grub-install --target=x86_64-efi --efi-directory=efi --removable

  # Or fix a previous install:
  mv /efi/EFI/GRUB /efi/EFI/BOOT
  mv /efi/EFI/BOOT/grubx64.efi /efi/EFI/BOOT/BOOTX64.EFI

  ```

  #### MBR
  ```bash
  pacman -S grub os-prober
  grub-install --target=i386-pc /dev/sdx
  grub-mkconfig -o /boot/grub/grub.cfg
  ```

</details><br>

## Reboot
```bash
exit
umount -R /mnt
reboot
```

## Post-install

Enable multilib repository:  
Uncomment the following two lines from `/etc/pacman.conf`:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

TODO: Uncomment Color in /etc/pacman.conf file.

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

# Uncomment the line:
%wheel ALL=(ALL:ALL) ALL

# Uncomment the line:
Defaults env_keep += "HOME"
```

### Add user to group
```bash
groups                          # Check your groups
groups <user>                   # Check user's groups
sudo groupadd <group>           # Create new group
sudo gpasswd -a <user> <group>  # Add user to group (needs relogin to take effect)
sudo gpasswd -d <user> <group>  # Remove user from group
sudo groupdel <group>           # Delete a group
```

## Graphical Environment

```bash
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

Wayland with nvidia:
```bash
nano /etc/modprobe.d/nvidia_drm.conf
options nvidia_drm modeset=1 fbdev=1
```

### KDE

```bash
pacman -S plasma-meta kde-applications-meta sddm sddm-kcm
# Choose qt6-multimedia-ffmpeg, pipewire-jack, noto-fonts, noto-fonts-emoji, phonon-qt5-vlc, pyside6, cronie, tesseract-data-eng if asked
systemctl enable sddm # Check if needed, it gave a warning last time
localectl set-x11-keymap pt
# If it doesn't work (login screen shows US keyboard layout even when user starts typing password)
nano /usr/share/sddm/scripts/Xsetup
setxkbmap pt
```

### Printing

```bash
sudo pacman -S cups cups-pdf print-manager hplip system-config-printer
sudo systemctl enable cups.service
```

### Additional Packages (Old)

#### Pacman
```bash
latte-dock
materia-kde kvantum-theme-materia
kvantum-qt5
papirus-icon-theme
kio-gdrive # Access Google Drive through Network section on Dolphin
```

#### AUR
```bash
breeze-blurred-git
plasma5-applets-eventcalendar
```

### Settings (new)

- Keyboard > Keyboard
  - Hardware > Delay: 200
  - Layouts > Configure Layouts > Add > pt
- Display & Monitor > Scale: 100%

### Settings

Konsole > Settings > Configure Konsole > Tab Bar / Splitters > Appearance > Position > Above

#### System Settings
- Appearance > Fonts > 8pt on everything
- Workspace Behavior
  - Desktop Effects
    - Translucency > Menus > 9
    - Blur
      - Blur > 10
      - Noise > 6
  - General Behavior > Double-click
  - Screen Locking > Automatically (off)
  - Virtual Desktops
    - Configure as desired (1 Row or 2 Rows)
    - Navigation wraps around (off)
- Window Management
  - Window Behavior > Advanced > Window placement > Centered
  - Kwin Scripts > MinimizeAll (on)
- Shortcuts > Global Shortcuts > KWin
  - Switch One Desktop Down > Ctrl + Alt + Down
  - Switch One Desktop to the Left > Ctrl + Alt + Left
  - Switch One Desktop to the Right > Ctrl + Alt + Right
  - Switch One Desktop Up > Ctrl + Alt + Up
  - Window One Desktop Down > Ctrl + Alt + Shift+ Down
  - Window One Desktop to the Left > Ctrl + Alt + Shift+ Left
  - Window One Desktop to the Right > Ctrl + Alt + Shift+ Right
  - Window One Desktop Up > Ctrl + Alt + Shift+ Up
- Startup and Shutdown > Login Screen (SDDM)
  - Theme > Choose something and go back to Breeze
  - Advanced > Automatically log in (if you want it)
  - Splash Screen > None
- Personalization
  - Notifications > Do not disturb when screens are mirrored (off)
  - Users > <user> > Photo
  - Regional Settings > Formats (Choose the same Region as the language and then override the Detailed Settings, otherwise some parts of the UI may be unwantedly translated)
- Hardware
  - Input Devices > Layouts
  - Audio > Applications > Notication Sounds -> Move slider somewhere and back to zero (initial zero is lying) # This will disable system sound events
  - Power Management > Energy Saving > Screen Energy Saving (40 min)
  - Printers

#### Panel

Right click > Edit Panel

- Screen Edge > Top
- Height: 28
- Remove Pager
- Remove Icons-only Task Manager
- Remove Application Launcher
- Remove Digital Clock
- Add Global Menu
- Add Spacer
- Add Event Calendar

Configure Event Calendar:
- Time in bold
- Enable Line 2, with 44% height
- Google Calendar

#### Latte Dock

Right click > Dock Settings

Remove Analog Clock

Items: 48px, Zoom: 50%
Background: 100%, Opacity: 30%

Right click > Add Widgets

- Application Launcher
  - Right click > Configure Application Launcher > Icon > "distributor"

Right click > Layouts > Configure > Preferences > Press Meta to activate Application Launcher

#### System tray

Right click > Configure System Tray

Entries:
- Clipbard (off)
- Instant Messaging (off)
- Vaults (off)
- Volume Control (hidden)

#### Calendar Widgets

1. Configure the Google Calendar account in KOrganizer
2. Add the digital clock widget and configure it using the PIM plugin
3. The events should now appear in the Calendar widget as well

#### Chrome
- Right click on title bar > Use system title bar and borders (off)

```bash
nano ~/.config/chrome-flags.conf

--force-dark-mode
--enable-features=WebUIDarkMode
```

#### Kvantum Manager

Themes:

https://github.com/Akava-Design/Akava-Kv

- Kvantum Manager > Install/Update Theme > Select a Kvantum theme folder > Install this theme
- Kvantum Manager > Change/Delete Theme > Akava-Kv > Use this theme
- Kvantum Manager > Configure Active Theme > Hacks > Transparent Dolphin view

- System Settings > Application Style > Application Style > kvantum-dark

- System Settings > Application Style > Window Decorations > BreezeBlurred > Configure
  - General
    - Draw titlebar background gradient (off)
    - Configure > Draw separator between titlebar and window (off)
    - Configure > Opacity (40%)
  - Shadows: Medium, 50%

https://github.com/Akava-Design/Akava-Theme

`cp -r Akava ~/.local/share/plasma/desktoptheme`

- System Settings > Appearance > Plasma Style > Akava

**Note:** Breeze Dark seems to work better.

https://github.com/Akava-Design/Akava-Colors

`cp Akava.colors ~/.local/share/color-schemes/`

- System Settings > Appearance > Colors > Akava

https://github.com/Akava-Design/Akava-Konsole

`cp Akava.colorscheme ~/.local/share/konsole`

- Konsole > Settings > Edit Current Profile > Appearance > Color scheme & font > Akava
- Settings > Edit Current Profile > Appearance > Edit > Blur background (on)
- Settings > Edit Current Profile > Appearance > Edit > Background transparency (25%)

<br><details>
  <summary>GNOME</summary>

  ### GNOME

  ```bash
  pacman -S gnome gnome-extra chrome-gnome-shell

  systemctl enable gdm.service
  systemctl enable NetworkManager.service
  ```

  When using nvidia proprietary drivers, it may be needed to uncomment the line `#WaylandEnable=false` in `/etc/gdm/custom.conf`.

  ### Printing

  ```bash
  sudo pacman -S cups cups-pdf ghostscript gsfonts hplip system-config-printer
  sudo systemctl enable org.cups.cupsd.service
  sudo systemctl start org.cups.cupsd.service
  ```

  Remember to go to "Printer > Configure" (or Print Settings application, right-click on printer) and set (default) properties.

  ### Additional packages

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


  ### Settings 

  #### Settings application
  - Region & Language > Input Sources > Your desired keyboard
  - Details > Date & Time > Automatic Date & Time / Time Zone
  - Details > Users > Photo


  #### Gnome tweak tool
  - Appearance > Themes > Applications > Arc-Dark
  - Appearance > Themes > Icons > Arc
  - Desktop > Icons on Desktop
  - Top Bar
  - Windows > Titlebar Buttons

  #### Dash-to-dock (right click on dock's Applications icon)
  - Position and size > Icon size: 32
  - Launchers > Move the applications button at the beginning of the dock
  - Appearance > Shrink the dash
  - Appearance > Show windows counter indicators
  - Appearance > Customize opacity (70%)

  #### Other applications
  - atom: config.cson and keymap.cson
  - terminal: Profile Preferences > Colors > White on Black

  #### Templates
  ```bash
  touch ~/Templates/Text\ Document.txt
  ```

  #### Alt-tab switch only between workspace apps
  ```bash
  gsettings set org.gnome.shell.app-switcher current-workspace-only true
  ```
</details><br>

## Other programs

### Pacman packages
```bash
openssh
steam
gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly gst-libav  # Video codecs
lib32-gst-plugins-base lib32-gst-plugins-good # Wine video codecs
```

### AUR packages
```bash
paru/yay                   # (manual installation)
google-chrome
visual-studio-code-bin     # For Settings Sync to work in KDE, you may need pacman -S gnome-keyring
tuxguitar
lib32-gst-plugins-bad li32-gst-plugins-ugly # Wine video codecs
```

### Reaper
```
sudo pacman -S realtime-privileges audacity
sudo gpasswd -a <USER> realtime
yay -S reaper-bin
# Logout and login
# In audacity, check which "hw" device is the capturing one (e.g. hw:2)
jackd -d alsa -d hw:2
```


### Syncthing

```bash
pacman -S syncthing
systemctl --user enable --now syncthing.service
```

Web interface is available at http://localhost:8384/


### LibreOffice
```bash
sudo pacman -S libreoffice-fresh    # Or libreoffice-still for stable version
```

If using a dark theme:
- Tools > Options > View > Icon style > Breeze (dark) 


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

## Bluetooth
```bash
sudo pacman -S bluez bluez-utils
# If bluetooth icon is not present:
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

### Logitech MX Master 3 Mouse

```bash
yay -S logiops-git
# Instructions: https://github.com/PixlOne/logiops/wiki/Configuration
sudo nano /etc/logid.cfg

devices: (
{
    name: "Wireless Mouse MX Master 3";
    smartshift:
    {
        on: true;
        threshold: 10;
    };
    hiresscroll:
    {
        hires: true;
        invert: false;
        target: false;
    };
    dpi: 1000;

    buttons: (
        {
            cid: 0xc3;
            action =
            {
                type: "Gestures";
                gestures: (
                    {
                        direction: "Up";
                        mode: "OnRelease";
                        action =
                        {
                            type: "Keypress";
                            keys: ["KEY_LEFTCTRL", "KEY_LEFTALT", "KEY_DOWN"];
                        };
                    },
                    {
                        direction: "Down";
                        mode: "OnRelease";
                        action =
                        {
                            type: "Keypress";
                            keys: ["KEY_LEFTCTRL", "KEY_LEFTALT", "KEY_UP"];
                        };
                    },
                    {
                        direction: "Left";
                        mode: "OnRelease";
                        action =
                        {
                            type: "Keypress";
                            keys: ["KEY_LEFTCTRL", "KEY_LEFTALT", "KEY_RIGHT"];
                        };
                    },
                    {
                        direction: "Right";
                        mode: "OnRelease";
                        action =
                        {
                            type: "Keypress";
                            keys: ["KEY_LEFTCTRL", "KEY_LEFTALT", "KEY_LEFT"];
                        };
                    },
                    {
                        direction: "None"
                        mode: "OnRelease";
                        action =
                        {
                            type: "Keypress";
                            keys: ["KEY_LEFTCTRL", "KEY_F10"];
                        };
                    }
                );
            };
        }
    );
}
);


sudo systemctl enable logid
sudo systemctl start logid
```

### Dualshock 3 / SIXAXIS

(After installing the bluetooth stack above)

```bash
sudo pacman -S bluez-plugins
```

- Reboot
- Connect the dualshock 3 using an USB cable, press the Home button
- Should see an authorization request, click Trust and Authorize
- Unplug the controller and press the Home button

If the request doesn't appear, try:

```bash
bluetoothctl
agent on
default-agent
power on
discoverable on
pairable on
# Connect the dualshock 3 using an USB cable, press the Home button
devices
# Unplug the controller
Should see a MAC Address, maybe an authorization request that you need to accept
trust <mac>
quit
```

## Change DNS servers

```bash
sudo nano /etc/resolv.conf
# Comment out the existing nameserver line and replace it with:
nameserver 8.8.8.8
nameserver 8.8.4.4
# The settings will probably be overwritten at reboot, add write-protection to the file if needed:
sudo chattr +i /etc/resolv.conf
```

## Wake on LAN (WoL)

```bash
sudo pacman -S ethtool
ip link   # Discover name of interface (e.g. enp2s0)
sudo ethtool enp2s0 | grep Wake   # d means disabled, g means magic packet (the required one)
sudo ethtool -s enp2s0 wol g

# Make persistent:
sudo su
cp /usr/lib/systemd/network/99-default.link /etc/systemd/network/50-wired.link
echo WakeOnLan=magic >> /etc/systemd/network/50-wired.link
exit
```



## Troubleshooting

View logs:
```bash
dmesg           # Kernel ring buffer logs
journalctl -b   # Boot log
journalctl -k   # Kernel log
```

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
