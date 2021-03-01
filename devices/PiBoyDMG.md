# PiBoy DMG

## Button mapping

Recommended button mapping maps Z to left trigger and C to right trigger, while the back buttons are tasked with the left and right shoulder buttons.

Button ids:

| Button       | ID | Button       | ID |
| ------------ | -- | ------------ | -- |
| Dpad up      | 12 | Analog up    | 1- |
| Dpad down    | 11 | Analog down  | 1+ |
| Dpad left    | 13 | Analog left  | 0- |
| Dpad right   | 14 | Analog right | 0+ |
|              |    | Analog click | 10 |
| A            | 0  | X            | 3  |
| B            | 1  | Y            | 4  |
| C            | 2  | Z            | 5  |
| L            | 6  | R            | 7  |
| Start        | 9  | Select       | 8  |


### PPSSPP

Edit file `/opt/retropie/emulators/ppsspp/assets/gamecontrollerdb.txt` and add:

```
15000000010000000100000000010000,PiBoy DMG Controller,platform:Linux,a:b1,b:b0,x:b4,y:b3,back:b8,start:b9,guide:b10,leftshoulder:b7,rightshoulder:b6,dpup:b12,dpdown:b11,dpleft:b13,dpright:b14,leftx:a0,lefty:a1,lefttrigger:b5,righttrigger:b2,
```

Then launch a PSP game, go to Settings (Analog click) and remap the controller L and R (replace pad buttons but don't delete keyboard mappings). You can add Z and C as additional mappings too.


## Fix sound

If sound crackles and pops after enabling OMX Player, change Sound Settings > OMX Player Audio Device to ALSA.

(Audio Card should be Default and Audio Device should be PCM, I think)

## Customize OSD (on-screen display)

Edit file `/home/pi/osd/osd.cfg`

### Configure LEDs and Fan

```
nano /boot/osd.cfg
redled
10

greenled
10

fanduty
4
61000 0
65000 75
70000 127
75000 200
```

## Configure Wifi

Create file /boot/wifikeyfile.txt with contents (replacing the values between quotes):
```
ssid="wifi_name"
psk="password"
```

Navigate to the "Retropie" menu.

Select the "Wifi" menu option.

Select "Import wifi credentials from /boot/wifikeyfile.txt" from the menu.

## Update firmware

```
# Convert loader.py from CRLF to LF
# Then:
chmod +x loader.py
sudo pacman -S python-pip
sudo pip install pyserial
sudo ./loader.py /dev/ttyACM0
```

## Update OSD scripts

1. Copy the update to /boot (contents should include payload folder and ask to overwrite config.txt, possibly others)
2. Edit `cmdline.txt` to add the init section: `init=/bin/bash -c "mount -t proc proc /proc; mount -t sysfs sys /sys; mount /boot; source /boot/unattended"`
3. Edit options in one-time-script.conf to setup wifi settings. Alternatively connect an ethernet cable.
4. Turn on the PiBoy, it should reboot about 5 times and take about 10 minutes.

