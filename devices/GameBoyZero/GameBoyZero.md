# Game Boy Zero

Game Boy Zero is a DIY handheld console done inside a Game Boy housing with a Raspberry Pi Zero.

These instructions are for a variant using a Raspberry Pi 3, for better performance.

## Original project site and forums

- [SudoMod](https://sudomod.com/)
- [Sota's variant](https://sudomod.com/forum/viewtopic.php?f=9&t=1615)
- [Info on screens](https://www.sudomod.com/forum/viewtopic.php?f=8&t=15&hilit=gearbest&start=520)
- [Screen performance](https://www.sudomod.com/forum/viewtopic.php?f=8&t=2850)
- [Known screens](https://www.sudomod.com/wiki/index.php/GBZ_Screen)

## Other resources

- [RPi pinout](http://pinout.xyz)

## Parts list

| Parts                                   | Price   |
| --------------------------------------- | ------- |
| Raspberry Pi 3                          | 36€     |
| Original Game Boy Housing (DMG Housing) | 4€      |
| Additional Game Boy buttons             | 3€      |
| 2x Game Boy conductive rubber           | 6€      |
| Game Boy DMG 4 button PCB               | 10€     |
| [Gearbest 3.5'' screen](https://www.gearbest.com/development-boards/pp_29447.html) | 17€ |
| **Total**                               | **59€** |


## GearBest screen (new)

RCA connector order (left to righ):

1. Red (5V)
2. Black (GND)
3. White (Sound, do not use)
4. Yellow (Video)

- Red: Solder to pin 2 or 4
- Black: Solder to pin 6, 9, 14, 20, 25, 30, 34 or 39
- Yellow: Solder to PP24

Remember to keep the black insolating rubber whenever possible, to shield the AV2 connection from VCC interference.
Alternatively, redo the connections keeping AV2 and GND together and shielded, with VCC outside.

If the screen displays poorly, try disabling the screen's voltage regulator from 12V to 5V, by shorting one of the pins of a chip:

![Gearbest mod](gearbest-screen.jpg)


## Housing

1. Enlarge the screen area so the 3.5'' screen fits.
2. Open space for the X and Y buttons.
3. Cut away the battery frame so that the Raspberry Pi 3 fits.


## Software

## Configuration

Edit /boot/config.txt and uncomment/modify the following lines:
```
hdmi_force_hotplug=1
```

### RetroPie

https://retropie.org.uk/

Extract retropie*.img.gz

```bash
# Discover which device is the SD card:
lsblk -p`
# (Unmount all partitions of the SD Card)
# Flash the image:
dd bs=4M if=retropie-buster-4.6-rpi2_rpi3.img of=/dev/sdX conv=fsync
```

Configure it with a keyboard and screen.

- Make sure to run raspi-config in the Configuration Menu:
  - Default locale
  - Wifi Country
  - Keyboard layout
  - Interfacing options > SSH > Enable > Change Password > Reboot
    - SSH hostname is `retropie`
- Wifi
- ES Themes: rxbrad/gbz35-dark (Change it afterwards in EmulationStation UI Settings)
- Run Command Configuration > Disable Run Command Menu




Keys:

- Hold any button to skip a particular button.
- Hotkey Enable is the equivalent of the Home button, to enable EmulationStation shortcuts. Leave it undefined to use Select.
  - Shorcuts:
    - Hotkey+Start = exit emulator
    - Hotkey+Right shoulder = save state
    - Hotkey+Left shoulder = load saved state
    - Hotkey+Left = decrease current saved state slot number
    - Hotkey+Right = increase current saved state slot number
    - Hotkey+X = quick menu (with access to most of these other items)
    - Hotkey+B = reset game

`TODO theme`

`TODO resolution`

Roms:

- Add files in ~/RetroPie/roms/<CONSOLE>



### Buttons

1. Get [Adafruit's retrogame](https://github.com/adafruit/Adafruit-Retrogame) to translate GPIO buttons to keyboard events
2. Copy retrogame to the retropie
3. Copy the correct configuration to /boot/retrogame.cfg:

`TODO cfg`
