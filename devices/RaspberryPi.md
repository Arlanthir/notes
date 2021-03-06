# Raspberry Pi

## Install Raspbian

```bash
sha256sum 2017-11-29-raspbian-stretch.zip # Checksum
unzip 2017-11-29-raspbian-stretch.zip

# Find SD Card:
df -h # Check mounted devices
# Insert SD Card
df -h # Look for new mounted device (eg: /dev/sdb1)

# Alternative:
sudo fdisk -l

umount /dev/sdb1
umount /dev/sdb2

sudo dd bs=4M if=2017-11-29-raspbian-stretch.img of=/dev/sdb status=progress conv=fsync

# sudo kill -USR1 $(pgrep ^dd$) # Monitor progress
# sudo sync # Ensure write before unplugging
```

## Change SSH port
Edit `/etc/ssh/sshd_config`
```
bash
Port 22   # Uncomment and choose another number
```

Default username/password: pi / raspberry

Make sure to change the password (and other configuration) in `sudo raspi-config`

## Disable overscan

If HDMI output doesn't match the edges of the screen:

```bash
sudo nano /boot/config.txt
disable_overscan=1
```

Alternatively, change it in `sudo raspi-config`.

## Overclocking

```bash
sudo nano /boot/config.txt

arm_freq=2000   # Up to 2147. Default is 1500.
v3d_freq=750    # Default is 500.
over_voltage=6
```

## Install a USB disk

Newest:
```bash
sudo apt-get install ntfs-3g   # If NTFS

lsblk -o NAME,MOUNTPOINT,UUID,PARTUUID  # Grab PARTUUID, e.g. 9712703b-01

sudo mkdir /mnt/usb
sudo mount /dev/disk/by-partuuid/<partuuid> /mnt/usb

# Add to /etc/fstab:
# For NTFS drives:
PARTUUID=<partuuid>  /mnt/usb  ntfs  defaults,auto,users,rw,nofail,umask=000  0  0
# For EXT4 drives:
PARTUUID=<partuuid>  /mnt/usb  ext4  defaults,auto,users,rw,nofail  0  0

# To reduce the timeout and allow faster boots when drive is not present, add flag:
,x-systemd.device-timeout=10
```


New:
```bash
sudo apt-get install ntfs-3g
sudo blkid   # Check the UUID of your device, something like E47AFD7A7AFD49B6
sudo mkdir /mnt/usb
sudo chmod 775 /mnt/usb
sudo mount /dev/disk/by-uuid/E47AFD7A7AFD49B6 /mnt/usb

# Add to /etc/fstab:
/dev/disk/by-uuid/E47AFD7A7AFD49B6   /mnt/usb    ntfs    defaults,permissions    0    0
```

Old:
```bash
sudo apt-get install ntfs-3g
sudo fdisk -l    # Check the name of your device, probably /dev/sda1
sudo mkdir /mnt/usb
sudo chmod 777 /mnt/usb
sudo mount /dev/sda1 /mnt/usb

# Add to /etc/fstab:
/dev/sda1    /mnt/usb    ntfs    defaults,permissions    0    0
```

## Install gaming machine

It may be required to split 256MB to GPU Memory.

```
sudo apt-get install git lsb-release steamlink
cd
git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git
cd RetroPie-Setup
chmod +x retropie_setup.sh
sudo ./retropie_setup.sh
# Choose Basic Install

# Other opt packages to consider: dosbox, kodi, lr-ppsspp/ppsspp, scraper
```

### Swap A/B in EmulationStation

On retropie_setup: `Manage packages` > `Manage core packages` > `emulationstation` > `Configurations/Options` > `Swap A/B Buttons in ES`

### Theme

In the `RetroPie` "emulator", go to the themes sub-app and install the theme gallery.
Good themes: `nostalgic`, `tronkyfran`, `retrorama`, `spaceoddity`, `magazinemadness`, `CosmicRise`, `simple`, `simple-dark`

### Disable game loading / boot screen

Retropie settings > Run Command > Launch Menu


### DualShock 3

On retropie_setup: `Manage Packages` > `drivers` > `sixaxis` > `Install from source`

Go to Options and configure the controller timeout.

Exit RetroPie Setup and return to the EmulationStation Home Screen.

Plug the controller in using the cable.

Launch the `RetroPie` "emulator". Select `Bluetooth` > `Register and Connect to Bluetooth Device` and follow the instructions.  
Once successfully paired, exit the Bluetooth menu.

Using the RetroArch RGUI (Hotkey + Y) you can configure the Gamepad to use Analog stick to D-Pad in the emulators of your choosing. Save the Core Remap afterwards. To change it for a single game, save the Game Remap instead.

You can also configure the device to register as Dualshock in the PSX emulator, and enable rumble. Again, save the Core Remap file afterwards. Some PSX games stop getting input because they don't support Dualshock controllers, so select the Standard controller in those and save it as a Game Remap file.

In the Core Options, outside the Quick Menu, Settings > Input > Analog Sensitivity: 1.1, Haptic Feedback/Vibration > Enable: ON. (Then Save)


### ROMS

Place ROMS in the appropriate folder inside /home/pi/RetroPie/roms.
BIOS files (e.g. PlayStation) go in /home/pi/RetroPie/BIOS.

#### Generating missing .cues

https://nielsbuus.dk/pg/psx_cue_maker/

Seems to always output something like:

```
FILE "<FILE>.img" BINARY
  TRACK 01 MODE2/2352
    INDEX 01 00:00:00
```

#### Converting between image types in Arch:

```bash
yay -S mame-tools ecm-tools ccd2cue mdf2iso

ecm2bin <file.bin.ecm>                         # Decompresses CD image (.bin, .img)
mdf2iso --cue <file.mdf>                       # Converts .mdf/.mds to .bin/.cue
ccd2cue -o <file.cue> <file.ccd>                # Converts .ccd to .cue
chdman createcd -i <game.cue> -o <game.chd>   # Converts .bin/.cue to .chd (smaller)
# In a loop:
for i in *.cue; do chdman createcd -i "$i" -o "${i%.*}.chd"; done
```

#### Multi-disc ROMs in one file

For multi-disc games on .cue & .bin ROM pairs or .chds, you can create a .m3u playlist file to enable you to change discs using the RGUI. Replace the .cue or .chd extension for each disc of the game with an appropriate .CD1, .CD2, etc so that EmulationStation will list only the .m3u and not the individual discs.

Example for Final Fantasy VII:

Folder Structure:

```bash
Final Fantasy VII (USA) (Disc 1).CD1    # This is the renamed .cue or .chd file.
Final Fantasy VII (USA) (Disc 2).CD2    # This is the renamed .cue or .chd file.
Final Fantasy VII (USA) (Disc 3).CD3    # This is the renamed .cue or .chd file.
Final Fantasy VII (USA).m3u
```

Final Fantasy VII (USA).m3u's Text Contents:

```
Final Fantasy VII (USA) (Disc 1).CD1
Final Fantasy VII (USA) (Disc 2).CD2
Final Fantasy VII (USA) (Disc 3).CD3
```

Then, in-game:

`Quick Menu` > `Disk Control` > `Disk Cycle Tray Status`

### Using Steven's `scraper`

Exit EmulationStation.

`sudo ~/RetroPie-Setup/retropie_setup.sh.`

Install `scraper` from opt packages.

Manage Packages > Manage Optional Packages > scraper > Configuration / Options

Relevant Options:

- Thumbnails Only: Disabled
- Arcade Source: ArcadeItalia
- Console Source: theGamesDB (ScreenScraper is recommended but works poorly for me)
- ROM Names: theGamesDB
- Use rom folder for gamelist & images: Enabled
- Download Videos: Enabled
- Download Marquees: Enabled

Remember to enable videos and omxplayer in EmulationStation settings.

CLI version:

- Register at https://screenscraper.fr/ and change country and language.
- Optionally donate 10€ to get 5 additional scraper threads.

```bash
sudo apt-get install handbrake-cli

cd /home/pi/RetroPie/roms/gamegear
/opt/retropie/supplementary/scraper/scraper -append=true -console_src="gdb,ss" -download_marquees=true -download_videos=true -lang="en" -use_nointro_name=false -workers=4 -ss_user=<user> -ss_password=<pass>

cd /home/pi/RetroPie/roms/arcade
/opt/retropie/supplementary/scraper/scraper -append=true -mame=true -convert_videos=true -download_marquees=true -download_videos=true -lang="en" -use_nointro_name=false -workers=4
```

To manually re-encode ArcadeItalia videos:

```bash
cd ~/RetroPie/roms/arcade/images
mkdir converted
for f in *.mp4; do HandBrakeCLI -E copy:aac -e x264 -B 64 -r 25 --gain -20.0 -X 320 -q 20 -i "$f" -o "./converted/$f"; done
```

### PlayStation settings

Quick Menu > Options > Enhanced Resolution (Slow) > ON (Try it)
Quick Menu > Options > Enhanced Resolution (Speed Hack) > ON (Try it if slow)

(Then save the game/core configutation)


### Add SteamLink shortcut

Install via exp packages in retropie-setup, or:

```
mkdir ./RetroPie/roms/ports 
nano ./RetroPie/roms/ports/Steam.sh

#!/bin/bash
steamlink
```

Add the following to `/etc/emulationstation/es_systems.cfg`
```
  <system>
    <name>ports</name>
    <fullname>Ports</fullname>
    <path>/home/pi/RetroPie/roms/ports</path>
    <extension>.sh .SH</extension>
    <command>bash %ROM%</command>
    <platform>pc</platform>
    <theme>ports</theme>
  </system>
```

## Install transmission-daemon

```bash
sudo apt-get install transmission-daemon
sudo service transmission-daemon stop
sudo nano /etc/transmission-daemon/settings.json

"download-dir": /mnt/usb/Downloads,
"encryption": 2,
"peer-port": 51414,
"port-forwarding-enabled": true,
"ratio-limit-enabled": true,
"rpc-authentication-required": true,
"rpc-password": "CHOSEN PASSWORD",
"rpc-port": 9091,
"rpc-whitelist": "192.168.1.*",
"rpc-whitelist-enabled": false,    # don't restrict access to same network
"umask": 2,                        # decimal for 002. inverse of (rwx,rwx,r-x)

sudo chgrp debian-transmission -R /mnt/usb/Downloads/     # Or another folder
sudo chmod -R 775 /mnt/usb/Downloads
sudo usermod -a -G debian-transmission pi # Add user pi to group debian-transmission
sudo service transmission-daemon start
```

### Add torrents by CLI

`transmission-remote -n '<user>:<pass>' -a <file.torrent>`

## Playing video

- `omxplayer` is already installed on latest Raspbian.
- Decodes video in hardware.
- Requires 64mb of graphical ram, 128 for 1080p.

```bash
omxplayer -rb <video> --subtitles <subtitles>
```

If screen is black after playback:

`Alt+f2`, `Alf+f1`

### Function in .bashrc (like alias)
```bash
function omx { file="$*"; noextension="${file%.*}"; omxplayer -rb "$file" --subtitles "$noextension.srt" --vol -1200; }
```

Allows: `omx <movie>`


### Shortcuts

Key         | Function
------------|-----------------------------------
s           | Toggle subtitles
d           | Subtitle delay -250 ms
f           | Subtitle delay +250 ms
q           | Exit OMXPlayer
Space / p   | Pause/Resume
\-          | Decrease Volume
\+          | Increase Volume
Left Arrow  | Seek -30
Right Arrow | Seek +30
Down Arrow  | Seek -600
Up Arrow    | Seek +600


### Subtitles

Download subtitles automatically with [OpenSubtitlesDownload](https://github.com/emericg/OpenSubtitlesDownload)

Convert subtitles to UTF-8:

```bash
iconv -f LATIN1 -t UTF-8 in.srt > out.srt
```


## Install Syncthing

New:
```bash
sudo apt-get install syncthing
sudo systemctl start syncthing@pi.service
# Wait some time
sudo systemctl stop syncthing@pi.service
nano ~/.config/syncthing/config.xml

<gui enabled="true" tls="false" debugging="false">
        <address>0.0.0.0:8080</address>

sudo systemctl start syncthing@pi.service
# Access the web interface, change user/password
# Restart
sudo systemctl enable syncthing@pi.service
```


Old:

1. Download the ARM version
2. Copy the files to /home/pi/syncthing
3. Run syncthing once to create the config.xml file in ~/.config/syncthing
4. Exit the application
5. Change in ~/.config/syncthing: `0.0.0.0:8080`
6. `sudo cp Syncthing/etc/linux-systemd/system/syncthing@.service /lib/systemd/system`
   - Or /etc/systemd/system, /run/systemd/system, /usb/lib/systemd/system?
7. `sudo systemctl enable syncthing@<USER>.service`
8. `sudo systemctl start syncthing@<USER>.service`
9. `sudo systemctl status syncthing@<USER>.service`

Access the URL with a remote computer and change configs: user (example: admin) and pass.

Use port forwarding on your router, port 8080 and possibly 22000

Remember to setup different ports if using other clients in the same home network.


### Install Sonarr

Copied from https://www.htpcguides.com/install-sonarr-raspberry-pi-2-latest-stable-mono/ (untested)

```
sudo apt-get install libmono-cil-dev
wget http://sourceforge.net/projects/bananapi/files/mono_3.10-armhf.deb # or http://www.frickelzeugs.de/mono_3.10-armhf.deb or https://www.dropbox.com/s/k6ff6s9bfe4mfid/mono_3.10-armhf.deb
sudo dpkg -i mono_3.10-armhf.deb
# may be needed: sudo apt-get install apt-transport-https -y --force-yes
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA5DFFC
echo "deb https://apt.sonarr.tv/ master main" | sudo tee -a /etc/apt/sources.list.d/sonarr.list
sudo apt-get update
sudo apt-get install nzbdrone sqlite3-dev -y
sudo chown -R pi:pi /opt/NzbDrone
```

Autostart:
```
sudo nano /etc/init.d/nzbdrone

#! /bin/sh
### BEGIN INIT INFO
# Provides: NzbDrone
# Required-Start: $local_fs $network $remote_fs
# Required-Stop: $local_fs $network $remote_fs
# Should-Start: $NetworkManager
# Should-Stop: $NetworkManager
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: starts instance of NzbDrone
# Description: starts instance of NzbDrone using start-stop-daemon
### END INIT INFO

############### EDIT ME ##################
# path to app
APP_PATH=/opt/NzbDrone

# user
RUN_AS=pi

# path to mono bin
DAEMON=$(which mono)

# Path to store PID file
PID_FILE=/var/run/nzbdrone/nzbdrone.pid
PID_PATH=$(dirname $PID_FILE)

# script name
NAME=nzbdrone

# app name
DESC=NzbDrone

# startup args
EXENAME="NzbDrone.exe"
DAEMON_OPTS=" "$EXENAME

############### END EDIT ME ##################

NZBDRONE_PID=`ps auxf | grep NzbDrone.exe | grep -v grep | awk '{print $2}'`

test -x $DAEMON || exit 0

set -e

#Look for PID and create if doesn't exist
if [ ! -d $PID_PATH ]; then
mkdir -p $PID_PATH
chown $RUN_AS $PID_PATH
fi

if [ ! -d $DATA_DIR ]; then
mkdir -p $DATA_DIR
chown $RUN_AS $DATA_DIR
fi

if [ -e $PID_FILE ]; then
PID=`cat $PID_FILE`
if ! kill -0 $PID > /dev/null 2>&1; then
echo "Removing stale $PID_FILE"
rm $PID_FILE
fi
fi

echo $NZBDRONE_PID > $PID_FILE

case "$1" in
start)
if [ -z "${NZBDRONE_PID}" ]; then
echo "Starting $DESC"
rm -rf $PID_PATH || return 1
install -d --mode=0755 -o $RUN_AS $PID_PATH || return 1
start-stop-daemon -d $APP_PATH -c $RUN_AS --start --background --pidfile $PID_FILE --exec $DAEMON -- $DAEMON_OPTS
else
echo "NzbDrone already running."
fi
;;
stop)
echo "Stopping $DESC"
echo $NZBDRONE_PID > $PID_FILE
start-stop-daemon --stop --pidfile $PID_FILE --retry 15
;;

restart|force-reload)
echo "Restarting $DESC"
start-stop-daemon --stop --pidfile $PID_FILE --retry 15
start-stop-daemon -d $APP_PATH -c $RUN_AS --start --background --pidfile $PID_FILE --exec $DAEMON -- $DAEMON_OPTS
;;
status)
# Use LSB function library if it exists
if [ -f /lib/lsb/init-functions ]; then
. /lib/lsb/init-functions
if [ -e $PID_FILE ]; then
status_of_proc -p $PID_FILE "$DAEMON" "$NAME" && exit 0 || exit $?
else
log_daemon_msg "$NAME is not running"
exit 3
fi

else
# Use basic functions
if [ -e $PID_FILE ]; then
PID=`cat $PID_FILE`
if kill -0 $PID > /dev/null 2>&1; then
echo " * $NAME is running"
exit 0
fi
else
echo " * $NAME is not running"
exit 3
fi
fi
;;
*)
N=/etc/init.d/$NAME
echo "Usage: $N {start|stop|restart|force-reload|status}" >&2
exit 1
;;
esac

exit 0
```

```
sudo chmod +x /etc/init.d/nzbdrone
sudo update-rc.d /etc/init.d/nzbdrone defaults 98 # or sudo update-rc.d nzbdrone defaults 98
```

Access the interface in http://ip.address:8989


## Increase boot timeout to allow `fsck`ing big disks

Edit `/etc/systemd/system.conf` (using another PC with an external SD Card reader if necessary).

Uncomment and change the value in the line:

```bash
#DefaultTimeoutStartSec=90s
```
