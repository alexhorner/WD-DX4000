# WD-DX4000
Running Ubuntu or Debian on the Western Digital WD Sentinel DX4000

---

![WD Sentinel DX4000](./img/wdnas.jpg?raw=true)

---

## 2021 Update - Ubuntu and Front Panel Controls
- Ubuntu is now available
- Work has begun on providing an interface for the entire front panel of the machine, including buttons, LCD (control, contract, brightness) and LEDs. Please visit the dedicated repo for more information and for a working proof of concept: https://github.com/alexhorner/WD-DX4000-IO

---

This guide will go through the entire process of running Ubuntu or Debian on the WD Sentinel DX4000 from start to finish.

## Prerequisites
- USB Keyboard
- USB Hub
- 1x 8GB USB flash drive (for the installer)
- 1x 8 or more GB (I use 32GB) USB flash drive (for permenant fixture as a boot drive)
- PuTTY installed (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Rufus installed (I recommend using 2.18 for compatibility https://github.com/pbatard/rufus/releases/download/v2.18/rufus-2.18.exe)
- Debian ISO downloaded (Get the small installation image 64-bit PC netinst iso https://www.debian.org/distrib/)
  
  OR

  Ubuntu ISO downloaded (Get the full server installer iso https://ubuntu.com/download/server)
- USB TTL serial adapter
- Soldering iron and wires to attach your TTL serial adapter to the pads on the board

## References
- Martin Meise for USB Installer setup, serial port layout etc https://www.youtube.com/watch?v=amUXkdGdcTg
- frant1c for UEFI stuff https://community.wd.com/t/howto-arch-linux-on-a-sentinel-dx4000-without-original-compatible-disks/204041/21

---

Head to [Before Install](BeforeInstall.md) to start!

## Other guides
- [Hard Drives](Disks.md)
- [Fan](Superio.md)
- [LCD](LCD.md)
- [Hardware](Hardware.md)

---

## Goals
- [Fan Control](Superio.md) - **COMPLETE**
- [LCD Writing](LCD.md) - **COMPLETE**
- LCD Contrast - (IN PROGRESS! See IO repo at top of this README) Works with manual control in default Windows
- [LCD Brightness](LCD.md) - **COMPLETE**
- Front Panel Button Reads - (IN PROGRESS! See IO repo at top of this README) Works with manual control in default Windows
- LED Control (RAID indicators and power indicators. Red and blue individually) - (IN PROGRESS! See IO repo at top of this README)  Pretty sure this is available as it is used by the default Windows software, but I couldn't work out how to control manually within default Windows

---

DISCLAIMER: I am not liable to any damages of any kind which are caused directly or indirectly by following this guide. You follow the provided instructions and use all provided information at your own risk, in the full knowledge that I am not required to assist you if something goes wrong (though if you ask I may try)

---

## Contact me!

If there is an issue with this repository in ANY way, such as a broken link, broken instruction in the guide, missing resource or ANYTHING else, please open an issue on the repository.

If you are interested in helping, please contact me via email at ahorner@programmer.net preferrably with some information on what you are capable of doing and i'll see what I can give you as something I cannot do. Of course I will give credit where credit is due!
