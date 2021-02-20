# PiBoy DMG

## Button mapping

Recommended button mapping maps Z to left trigger and C to right trigger, while the back buttons are tasked with the left and right shoulder buttons.

### PPSSPP

Edit file `/opt/retropie/emulators/ppsspp/assets/gamecontrollerdb.txt` and add:

```
15000000010000000100000000010000,PiBoy DMG Controller,platform:Linux,a:b1,b:b0,x:b4,y:b3,back:b8,start:b9,leftstick:b10,leftshoulder:b7,rightshoulder:b6,dpup:b12,dpdown:b11,dpleft:b13,dpright:b14,leftx:a0,lefty:a1,lefttrigger:b2,righttrigger:b5,
```

Then plug in a keyboard, launch a PSP game, Press ESC, go to Settings and remap the controller (replace pad buttons but don't delete keyboard mappings).

Relevant buttons: L, R (seem badly mapped) and pause (e.g. C), to be able to open the menu without a keyboard.

## Fix sound

If sound crackles and pops after enabling OMX Player, change Sound Settings > OMX Player Audio Device to ALSA.

(Audio Card should be Default and Audio Device should be PCM, I think)

## Customize OSD (on-screen display)

Edit file `/home/pi/osd/osd.cfg`

## Configure Wifi

Create file /boot/wifikeyfile.txt with contents (replacing the values between quotes):
```
ssid="wifi_name"
psk="password"
```

Navigate to the "Retropie" menu.

Select the "Wifi" menu option.

Select "Import wifi credentials from /boot/wifikeyfile.txt" from the menu.

## Configure LEDs and Fan

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
