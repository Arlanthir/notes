# Windows

## Disable fast startup

Control Panel > Hardware and Sound > Power Options > Choose what the power buttons do > Change settings that are currently unavailable > Turn on fast startup.

## Startup shortcuts location
Windows + R > `shell:startup`

## Taskbar shortcuts location
`%AppData%\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

## Repair UEFI Menu entry

Flash the Windows 10 Installation media onto a USB drive and boot it
Use it to "Troubleshoot, Repair, Command Line"

```bash
diskpart
list disk
select Disk 0
list partition
# Check if the first partition is a 260MB System partition
select partition 1
assign letter=U
list vol
# Confirm Windows drive is C and UEFI partition is U
exit
bcdboot C:\windows /s U: /f UEFI
wpeutil reboot
```

Remove the USB drive.