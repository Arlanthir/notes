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

- If sound crackles and pops after enabling OMX Player, change Sound Settings > OMX Player Audio Device to ALSA.
- In RetroPie 4.6-, EmulationStation Audio Card should be Default and Audio Device should be PCM.
- In RetroPie 4.7.1+, EmulationStation Audio Card should be Sysdefault and Audio Device should be Headphones/HDMI.
- EmulationStation sound level should always be 100%.


## Customize OSD (on-screen display)

Edit file `/boot/osd.cfg`

### Configure LEDs, Fan and text/icons

```bash
#classicaudio Use PCM instead of HDMI/HEADPHONE audio devices
#classicaudio

#loadwait sets an application to wait to open before closing the boot splash screen
#loadwait
#emulationstation

#red LED maximum brightness
redled
10

#green LED maximum brightness
greenled
10

#volumeicon enables displaying the volume slider when changing volume. 0 disables display.  1 thru 100 sets transparency(alpha).
volumeicon
50

#fanduty defines the number of temperature bands and fan power for each given temperature.  
#fanduty defaults to full speed if underfined. 
#temps are in millidegrees
#duty/power is in tenths of a percent.  0 to 254 = 0% to 25.4% duty. Setting 255 will set 100% power.
#on boot fan is set to 100% power until changed by osd or third party applications.
#fanduty
#6
#45000 0
#50000 75
#55000 110
#60000 147
#65000 194
#70000 242

#alternative fanduty
#fanduty
#6
#50000 0
#55000 20
#60000 35
#65000 50
#70000 70
#75000 90

fanduty
4
61000 0
65000 75
70000 127
75000 200

#statistics data can be printed on the screen
#load - shows combined processor load
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
load
50
0
5
medium

#temperature shows processor temp in degrees C
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
temperature
50
80
5
medium

#votlage shows estimated open circuit battery votlage
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
voltage
50
140
5
medium

#current shows battery current. Positive current is charging and negative current is discharging
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
current
50
190
5
medium

#cpu shows the current cpu usage percentage.  100% per core, eg 400% indicates all 4 cores are maxed.
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
cpu
50
240
5
medium

#icons
#throttle - will display a throttling icon if temperature throttling.
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
throttle
50
520
5
medium

#bluetooth - will display a bluetooth icon(idle or active) if bluetooth is enabled
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
bluetooth
50
550
0
medium

#wifi - will display a wifi connection quality icon if wifi is enabled
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
wifi
50
580
0
medium

#battery - will display a battery meter
#alpha 0 to 100
#x 0 to 639
#y 0 to 479
#size small, medium or large
battery
50
610
0
medium
```

## Overclock

Recommended overclock settings:

```bash
sudo nano /boot/config.txt

arm_freq=2000   # Up to 2147. Default is 1500.
v3d_freq=750    # Default is 500.
over_voltage=6
```

## Configure Wifi

Create file /boot/wifikeyfile.txt with contents (replacing the values between quotes):

```bash
ssid="wifi_name"
psk="password"
```

1. Navigate to the "Retropie" menu.
2. Select the "Wifi" menu option.
3. Select "Import wifi credentials from /boot/wifikeyfile.txt" from the menu.

## Update firmware (on Linux)

1. On your computer, download the firmware update, which should contain a `loader.py` file.
2. Connect your PiBoy while it's off to your PC (using a USB cable).
3. `cd` to the firmware update folder on your PC.
4. Convert `loader.py` from CRLF to LF.
5. Issue the commands:

```bash
chmod +x loader.py
sudo pacman -S python-pip      # Example for Arch linux
pip install --user pyserial
sudo ./loader.py /dev/ttyACM0
```

## Update OSD scripts

1. Copy the update to /boot (contents should include payload folder and ask to overwrite config.txt, possibly others)
2. Edit `cmdline.txt` to add the init section: `init=/bin/bash -c "mount -t proc proc /proc; mount -t sysfs sys /sys; mount /boot; source /boot/unattended"`
3. Edit options in one-time-script.conf to setup wifi settings. Alternatively connect an ethernet cable.
4. Optionally connect to an HDMI screen to see output, or hold start while booting to see it in the PiBoy's screen.
5. Turn on the PiBoy, it should reboot about 5 times and take about 10 minutes.

## Bootloader mode

Press the joystick in until it clicks and hold it down while turning the PiBoy on. Release the joystick button. The power LED should begin to flash red.

