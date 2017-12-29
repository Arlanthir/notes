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

sudo dd bs=4M if=2017-11-29-raspbian-stretch.img of=/dev/sdb conv=fsync

sudo kill -USR1 $(pgrep ^dd$) # Monitor progress

# sudo sync # Old way of syncing
```

Default username/password: pi / raspberry

## Install a USB disk

```bash
sudo apt-get install ntfs-3g
sudo fdisk -l    # Check the name of your device, probably /dev/sda1
sudo mkdir /mnt/usb
sudo chmod 777 /mnt/usb
sudo mount /dev/sda1 /mnt/usb

# Add to /etc/fstab:
/dev/sda1    /mnt/usb    ntfs    defaults,permissions    0    0
```

## Install Syncthing

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

## Install transmission-daemon

```bash
sudo apt-get install transmission-daemon
sudo service transmission-daemon stop
sudo nano /etc/transmission-daemon/settings.json

"download-dir": /mnt/usb/Downloads,
"encryption": 2,
"peer-port": 51414,
"port-forwarding-enabled": true,
"rpc-authentication-required": true,
"rpc-password": "CHOSEN PASSWORD",
"rpc-port": 9091,
"rpc-whitelist": "192.168.1.*",
"rpc-whitelist-enabled": false,    # don't restrict access to same network
"umask": 2,                        # decimal for 002. inverse of (rwx,rwx,r-x)

sudo chgrp debian-transmission -R /mnt/usb/Downloads/     # Or another folder
chmod -R 775 /mnt/usb/Downloads
sudo usermod -a -G debian-transmission pi # Add user pi to group debian-transmission
sudo service transmission-daemon start
```

1. Access the web UI
2. Click settings in the bottom left corner
3. Check: Set require encryption, another port (e.g. 51414) and port forwarding

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
function omx { file=$1; noextension=${file%.*}; omxplayer -rb $file --subtitles $noextension.str --vol -1200; }
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

