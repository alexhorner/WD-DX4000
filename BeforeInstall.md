# Installation Guide

## Part 1 - Serial Access

In order to follow this guide, BIOS access is a requirement. the DX4000 does not provide any video out, and the onboard PCIe also does not support any graphics card (that I have to hand and tested)

To get access to the BIOS, a small modification must be completed which is the addition of 3 flywires to the motherboard.

To begin, remove all drives from the DX4000 and the 5 rear screws on the case:

![Rear Case](./img/rear.jpg?raw=true)

With the case removed, turn the DX4000 anticlockwise 90 degrees, so that the front panel is touching your left hand and the rear IO your right hand.

Looking down upon the motherboard, on the top left there are pads for a serial port. Solder wires to the pads and connect them to your USB to Serial adapter like so:

![Serial Port](./img/serial.jpg?raw=true)

on the pads (J23) Black is ground, Red is TX, Grey is RX.

Connect your adapter to your computer and use Device Manager to identify the COM port, in my case COM3. Configure PuTTY to use your COM port at 115200 baud:

![PuTTY Configuration](./img/putty.jpg?raw=true)

## Part 2 - Choose Your OS
This guide explains the full installation process of Ubuntu and Debian. Please choose one of the guide files listed below to continue your installation:
- InstallDebian.md
- InstallUbuntu.md