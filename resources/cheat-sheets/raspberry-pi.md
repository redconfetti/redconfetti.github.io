---
layout: page
title: Raspberry Pi
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

## Command Line

```bash
# Start X-Windows
# Use CTRL+ALT+DELETE to access menu to exit X-Windows
startx

# Run Raspberry Pi Configuration Tool
sudo raspi-config

# Shutdown Immediately
shutdown -h now

# Reboot
sudo reboot
```

## WiringPi

These are commands for the [WiringPi] tool kit.

[WiringPi]: https://github.com/WiringPi/WiringPi

```bash
# read data from all GPIO pins
gpio readall
```
