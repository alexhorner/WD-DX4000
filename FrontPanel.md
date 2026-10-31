# Front panel controls

This page documents front-panel mappings verified on a WD Sentinel DX4000 running Debian Linux.

The mappings below were tested on real hardware by toggling each output individually and by inserting/removing disks from the physical bays. GPIO outputs listed here are active high (`1` = LED on, `0` = LED off).

## Drive bay LEDs

The blue activity and red fault LEDs for each drive bay are connected to Intel ICH GPIO lines:

| Bay | Blue LED | Red LED |
| --- | ---: | ---: |
| 1 | GPIO 16 | GPIO 18 |
| 2 | GPIO 20 | GPIO 32 |
| 3 | GPIO 33 | GPIO 34 |
| 4 | GPIO 49 | GPIO 55 |

The power LEDs are:

| Indicator | GPIO |
| --- | ---: |
| Power blue | GPIO 27 |
| Power red | GPIO 28 |

On this platform the ICH GPIO I/O base is `0x480`. The level registers used by these GPIOs are:

- `LVL` at `0x48c` for GPIO 0-31
- `LVL2` at `0x4b8` for GPIO 32 and above
- `BLINK` at `0x498`; the stock power LED configuration was observed with the GPIO 27 blink bit set

Recent Debian kernels may not provide the `gpio_ich` driver as a built module. The mappings can still be accessed through `/dev/port`, or through the Linux GPIO character device if `gpio_ich` is available for the running kernel.

## Physical bay to SATA port mapping

Linux block device names such as `/dev/sda` are not a reliable way to identify a physical bay. The DX4000 bay mapping is stable at the ATA-port level:

| Bay | ATA port |
| --- | --- |
| 1 | `ata3` |
| 2 | `ata4` |
| 3 | `ata5` |
| 4 | `ata6` |

This can be used to map a block device to a physical bay by resolving `/sys/class/block/<device>/device` and extracting the `ataN` component from the resulting path.

## Front panel buttons

The front-panel Up and Down buttons are exposed through the Nuvoton NCT6776 Super I/O controller.

The existing WD-DX4000-IO proof of concept identifies logical device 7 and register `0xE9` as the button-state register. Testing establishes the following values for the two relevant active-low bits:

| State | `E9 & 0x0c` |
| --- | ---: |
| No button pressed | `0x0c` |
| Up pressed | `0x04` |
| Down pressed | `0x08` |

To access the register through `/dev/port`:

1. Write `0x87` twice to port `0x4e` to enter Super I/O configuration mode.
2. Select register `0x07` at port `0x4e` and write `0x07` to port `0x4f` to select logical device 7.
3. Select register `0xe9` at port `0x4e`.
4. Read the value from port `0x4f` and mask it with `0x0c`.
5. Write `0xaa` to port `0x4e` when finished to leave configuration mode.

For reliable button handling, wait until the masked value returns to `0x0c` before accepting another press.

## LCD backlight

As described in [LCD.md](./LCD.md), NCT6776 PWM3 controls the LCD backlight. Setting `pwm3` to `0` switches the backlight fully off; values up to `255` increase brightness.

For example:

```sh
echo 0 > /sys/class/hwmon/hwmonX/pwm3
```

Do not rely on a fixed `hwmonX` number across boots. Locate the hwmon directory whose `name` file contains `nct6776`.
