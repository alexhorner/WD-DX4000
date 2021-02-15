# Ubuntu Installation Guide
NOTE: This guide expects you to have followed BeforeInstall.md! If you haven't, go do it!

## Part 1 - Ubuntu Installer

Connect your USB install drive and open Rufus. Configure Rufus like below:

![Rufus](./img/rufus.jpg?raw=true)

Select your Ubuntu Server ISO by clicking the CD Drive icon on the right under Format Options.

Ensure (triple check!) you have the correct device selected, because selecting the wrong device could wipe out another device with your data on it!

Click start. You will be asked what mode you wish to use:

![Rufus Mode](./img/rufusmode.jpg?raw=true)

Choose ISO mode and click OK.

![Rufus Warning](./img/rufuswarn.jpg?raw=true)

READ!! and accept the warning.

Now we need to make a modification to the boot menu of the installer. Open file explorer and navigate to the USB:

![USB Root](./img/usbrootubuntu.jpg?raw=true)

Navigate to `/boot/grub`:

![USB /boot/grub](./img/usbbootgrub.jpg?raw=true)

Open grub.cfg in your editor:

![GRUB Config](./img/grubcfg.jpg?raw=true)

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!On line 27, you can see the default boot entry:

```
menuentry --hotkey=g 'Graphical install' {
    set background_color=black
    linux    /install.amd/vmlinuz vga=788 --- quiet 
    initrd   /install.amd/gtk/initrd.gz
}
```

Add a new entry above it, and configure it like so:

```
menuentry --hotkey=g 'Serial Console install' {
    set background_color=black
    linux    /install.amd/vmlinuz vga=off --- console=ttyS1,115200n8
    initrd   /install.amd/gtk/initrd.gz
}
```

At the top of the config on line 10, set `terminal_output` to `gfxterm console`

Your config will now look like so:

![GRUB Config Modified](./img/grubcfgmodified.jpg?raw=true)

## Part 2 - Set up hardware

Ensure no hard disks are in the 4 bays of the DX4000.

Into USB port 1 plug your empty USB onto which we will install Debian.

Into USB port 2, plug in your USB hub. Plug your USB keyboard and your Debian install USB into the hub.

Plug in power on power port 1 and network on network port 1.

## Part 3 - BIOS configuration

With PuTTY open, press the power button. Spam the DEL key on the USB keyboard (NOT on your PuTTY window using your normal keyboard) You will be presented with the BIOS:

![Bios](./img/bios1.jpg?raw=true)

Go to Save & Exit and select `Restore Defaults`

![Bios Defaults](./img/bios2.jpg?raw=true)

Select `Save Changes and Exit`:

![Bios Exit](./img/bios3.jpg?raw=true)

Spam DEL again to enter the BIOS.

Go to Advanced and ensure the settings at the top are configured like so:

![Bios Advanced](./img/bios4.jpg?raw=true)

Under Advanced select `IDE Configuration` and modify the settings to match below:

![Bios Advanced IDE Configuration](./img/bios5.jpg?raw=true)

Hit ESC once to go back to the Advanced menu, select `Serial Port Console Redirection` and modify the settings to match below:

![Bios Advanced Serial Port Console Redirection](./img/bios6.jpg?raw=true)

Now select `Console Redirection Settings` and modify the settings to match below:

![Bios Advanced Serial Port Console Redirection - Console Redirection Settings](./img/bios7.jpg?raw=true)

Hit ESC twice to go back to the Advanced menu.

Scroll to Boot and modify the settings to match below:

![Bios Boot](./img/bios8.jpg?raw=true)

Please note that `Boot Option #1` must be configured to the UEFI mode of your Debian installer USB! All other boot options must be disabled.

Go to Save & Exit and select `Save Changes and Exit`:

![Bios Exit](./img/bios3.jpg?raw=true)

## Part 4 - Debian Installation

When your NAS reboots, you will be presented with the Debian Installer GRUB menu:

![Debian Installer GRUB](./img/afterbios.jpg?raw=true)

Place your USB keyboard aside (don't unplug). We are now going to use your normal keyboard with PuTTY.

Press enter on the `Serial Console install` option.

Select your language, in my case English, and press enter:

![Debian Installer Language](./img/debianinstall1.jpg?raw=true)

Select your location:

![Debian Installer Location](./img/debianinstall2.jpg?raw=true)

Select your keyboard layout:

![Debian Installer Keyboard Layout](./img/debianinstall3.jpg?raw=true)

Choose the first option for network:

![Debian Installer Network](./img/debianinstall4.jpg?raw=true)

Configure a hostname:

![Debian Installer Hostname](./img/debianinstall5.jpg?raw=true)

You can leave domain name blank:

![Debian Installer Domain Name](./img/debianinstall6.jpg?raw=true)

Configure a root password:

![Debian Installer Root Password](./img/debianinstall7.jpg?raw=true)

Confirm the root password:

![Debian Installer Root Password Confirmation](./img/debianinstall8.jpg?raw=true)

Configure a personal account full name:

![Debian Installer Personal Full Name](./img/debianinstall9.jpg?raw=true)

Configure a personal account username:

![Debian Installer Personal Username](./img/debianinstall10.jpg?raw=true)

Configure a personal account password:

![Debian Installer Personal Password](./img/debianinstall11.jpg?raw=true)

Confirm the personal account password:

![Debian Installer Personal Password Confirmation](./img/debianinstall12.jpg?raw=true)

You may be asked if you want to force UEFI installation. Choose YES:

![Debian Installer UEFI Force](./img/debianinstall13.jpg?raw=true)

When asked how you wish to use the disk, select `Guided - use entire disk`:

![Debian Installer Disk Layout](./img/debianinstall14.jpg?raw=true)

From the disk selection menu, select the disk you wish to install onto. This is the one placed into USB port 1:

![Debian Installer Disk Selection](./img/debianinstall15.jpg?raw=true)

For partitioning, select `All files in one partition`:

![Debian Installer Disk Partitioning](./img/debianinstall16.jpg?raw=true)

Confirm writing the partition table:

![Debian Installer Disk Partitioning Confirmation](./img/debianinstall17.jpg?raw=true)

Say `Yes` to writing changes to the disk:

![Debian Installer Disk Partitioning Write Confirmation](./img/debianinstall18.jpg?raw=true)

The installer will now proceed to spend a while installing everything. Once this has been completed you will be presented with the package management configuration. Choose a suitable server location for where you live:

![Debian Installer Package Manager Location](./img/debianinstall19.jpg?raw=true)

Choose a suitable server in your location:

![Debian Installer Package Manager Server](./img/debianinstall20.jpg?raw=true)

Leave the HTTP proxy empty (unless you know what you need to input) and press enter:

![Debian Installer Package Manager HTTP Proxy](./img/debianinstall21.jpg?raw=true)

Choose whether you wish to participate in package popularity data collection:

![Debian Installer Package Data Collection](./img/debianinstall22.jpg?raw=true)

Configure which packages you want to install by default. Use the up and down arrows to navigate and the space bar to enable and disable items. I disabled `Debian desktop environment` as we have no video out, print server because I hate printers, I enabled SSH server as I don't want to be forced to use the serial console and kept standard system utilities enabled as they're handy:

![Debian Installer Packages](./img/debianinstall23.jpg?raw=true)
![Debian Installer Packages](./img/debianinstall24.jpg?raw=true)

Now you'll need to wait a while whilst the installer finishes installing packages and configures the GRUB bootloader.

Once the installation has finished, you will be presented with the completion screen:

![Debian Installer Completed](./img/debianinstall25.jpg?raw=true)

Unplug the installation USB and press enter, the switch back to the USB keyboard and spam DEL once the system restarts to enter the BIOS once again.

You'll notice the BIOS looks much nicer due to the configuration changes we made earlier:

![BIOS After Install](./img/biosafter1.jpg?raw=true)

Go to Boot and configure the boot order to match below:

![BIOS After Install Boot Order](./img/biosafter2.jpg?raw=true)

Go to Save & Exit and select `Save Changes and Exit`:

![BIOS After Install Exit](./img/biosafter3.jpg?raw=true)

You will now boot into Debian! You will be presented with a console which you need to control with PuTTY and your normal keyboard.

You can log in and get your DX4000's IP for SSH with the `ip a` command like so:

![IP Address](./img/ipaddress.jpg?raw=true)

As you can see my DX4000's IP is `10.0.10.10` on my local network! Lets SSH to that (root SSH is disabled, see SSHd config to enable that):

![SSH](./img/ssh.jpg?raw=true)

## Complete!

For the sake of it, you can use the command `shutdown -h now` to shut your DX4000 down, then turn it back on yourself and see it spring to life with no intervention! One Debian server for you to run whatever you want!

Please return to README.md for links to other guides, such as setting up 4 hard drives with the 4 bays in the NAS!
