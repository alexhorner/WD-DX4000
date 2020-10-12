# Enabling Kernel support for the SuperIO chip

## Step 1 - Required software

The `lm-sensors` package is required. Install it with the `apt-get install lm-sensors` command.

The `fancontrol` package is also required. Install it with the `apt-get install fancontrol` command.

## Step 2 - Detection and enabling

Run the `sensors-detect` command.

When asked:
```
Some south bridges, CPUs or memory controllers contain embedded sensors.
Do you want to scan for them? This is totally safe. (YES/no):
```
Choose YES.


When asked:
```
Some Super I/O chips contain embedded sensors. We have to write to
standard I/O ports to probe them. This is usually safe.
Do you want to scan for Super I/O sensors? (YES/no):
```
Choose YES.


When asked:
```
Some hardware monitoring chips are accessible through the ISA I/O ports.
We have to write to arbitrary I/O ports to probe them. This is usually
safe though. Yes, you do have ISA I/O ports even if you do not have any
ISA slots! Do you want to scan the ISA I/O ports? (yes/NO):
```
Choose NO.


When asked:
```
Lastly, we can probe the I2C/SMBus adapters for connected hardware
monitoring devices. This is the most risky part, and while it works
reasonably well on most systems, it has been reported to cause trouble
on some systems.
Do you want to probe the I2C/SMBus adapters now? (YES/no):
```
Choose YES.


If the setup asks about `Next adapter: xxxx - Do you want to scan it?` always choose NO.

You should see a similar output to the one below:
```
Now follows a summary of the probes I have just done.
Just press ENTER to continue:

Driver `coretemp':
  * Chip `Intel digital thermal sensor' (confidence: 9)

Driver `nct6775':
  * ISA bus, address 0xa30
    Chip `Nuvoton NCT5573D/NCT5577D/NCT6776F Super IO Sensors' (confidence: 9)

To load everything that is needed, add this to /etc/modules:
#----cut here----
# Chip drivers
coretemp
nct6775
#----cut here----
If you have some drivers built into your kernel, the list above will
contain too many modules. Skip the appropriate ones!

Do you want to add these lines automatically to /etc/modules? (yes/NO)
```

Choose YES

Now run the `/etc/init.d/kmod start` command.

## Step 3 - Reboot

Simply run `shutdown -r now` and your system will reboot.

## Step 4 - Fan setup

To start the fan setup, run the `pwmconfig` command.

When warned:
```
Warning!!! This program will stop your fans, one at a time,
for approximately 5 seconds each!!!
This may cause your processor temperature to rise!!!
If you do not want to do this hit control-C now!!!
Hit return to continue:
```

Press enter.

It will run a test on each 'fan' one by one. Since the DX4000 only has 1 fan, 2 of the 3 'fans' which were detected won't do anything.

As the tests are run, the loud fan may either stay loud, or switch off. You will get this prompt several times:
```
Did you see/hear a fan stopping during the above test (n)?
```

If the loud fan switched off, type `y` and press enter, otherwise just press enter. Usually the first test has no effect, but the second test does.

When prompted:
```
Testing pwm control hwmon1/pwm2 ...
  hwmon1/fan2_input ... speed was 2284 now 0
    It appears that fan hwmon1/fan2_input
    is controlled by pwm hwmon1/pwm2
Would you like to generate a detailed correlation (y)?
```

Press enter to proceed.

The setup will now build a speed correlation with each PWM level, and then test fan 3. Of course, nothing will happen as theres only 1 fan. Just like before, answer this question as appropriate:
```
There is either no fan connected to the output of hwmon1/pwm3,
or the connected fan has no rpm-signal connected to one of
the tested fan sensors. (Note: not all motherboards have
the pwm outputs connected to the fan connectors,
check out the hardware database on http://www.almico.com/forumindex.php)

Did you see/hear a fan stopping during the above test (n)?
```

Now we can set up automated fan control. When prompted:
```
Testing is complete.
Please verify that all fans have returned to their normal speed.

The fancontrol script can automatically respond to temperature changes
of your system by changing fanspeeds.
Do you want to set up its configuration file now (y)?
```

Make sure the fan is running (if now, shutodwn and start this entire step again to prevent thermal damage!) and then press enter.

For `What should be the path to your fancontrol config file (/etc/fancontrol)?` press enter.

You will be asked to choose an option from this list:
```
Select fan output to configure, or other action:
1) hwmon1/pwm2
2) Change INTERVAL
3) Just quit
4) Save and quit
5) Show configuration
select (1-n):
```

Type 1 and press enter.

You will be presented with a similar list to this:
```
Devices:
hwmon0 is coretemp
hwmon1 is nct6776

Current temperature readings are as follows:
hwmon0/temp2_input      19
hwmon0/temp3_input      19
hwmon1/temp1_input      39
hwmon1/temp2_input      60
hwmon1/temp3_input      75
hwmon1/temp7_input      0
hwmon1/temp8_input      0
hwmon1/temp9_input      0

Select a temperature sensor as source for hwmon1/pwm2:
1) hwmon0/temp2_input
2) hwmon0/temp3_input
3) hwmon1/temp1_input
4) hwmon1/temp2_input
5) hwmon1/temp3_input
6) hwmon1/temp7_input
7) hwmon1/temp8_input
8) hwmon1/temp9_input
9) None (Do not affect this PWM output)

select (1-n):
```

Use the temperature reading list to choose the second hottest sensor, in my case thats `hwmon1/temp2_input` and press enter. I chose this sensor as it appears to change the most in my testing and appears to be the CPU temp. The hottest one appears to be a chassis sensor which doesn't really change no matter the fan speed.

For this question:
```
Enter the low temperature (degree C)
below which the fan should spin at minimum speed (20):
```

I entered 35

For this question:
```
Enter the high temperature (degree C)
over which the fan should spin at maximum speed (60):
```

I entered 90 as the box seems to run quite hot even with the fan on max (70s)

For this question:
```
Enter the minimum PWM value (0-255)
at which the fan STOPS spinning (press t to test) (100):
```

I pressed t to run autodetect test

For this question:
```
Enter the minimum PWM value (2-255)
at which the fan STARTS spinning (press t to test) (150):
```

I pressed t, and at this menu:
```
Now we increase the PWM value in 10-unit-steps.
Let the fan stop completely, then press return until the
fan starts spinning. Then enter 'y'.
We will use this value +20 as the starting speed.
Setting hwmon1/pwm2 to 2...
```

I pressed y then pressed enter as I could feel the fan running (press enter like it says until you can feel and see your fan running if the default isn't working right)

For this question:
```
Enter the PWM value (0-2) to use when the temperature
is below the low temperature limit (0):
```

I chose 2 to ensure the fan was always running a bit (though its inaudiable)

And for this question:
```
Enter the PWM value (2-255) to use when the temperature
is over the high temperature limit (255):
```

I pressed enter to keep the default

We now drop back to the selection menu:
```
Select fan output to configure, or other action:
1) hwmon1/pwm2
2) Change INTERVAL
3) Just quit
4) Save and quit
5) Show configuration
select (1-n):
```

I chose 4 to save and quit


## Step 5 - Enable fan control

Now we need to enable fan control at startup. Run the `systemctl enable fancontrol` command.

We can also start fan control immediately with `service fancontrol start`.

You can also reboot with `shutdown -r now` to test fan control enables at boot.