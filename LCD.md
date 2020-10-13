# LCD Control

**PLEASE NOTE**: This is missing bits. Although I know you can change the contrast of and the text on the LCD, I have not yet figured out a way to do so. I have however worked out how to control the bightness.

## Brightness

Ensure you have followed Superio.md in order to enable PWM control. This is usually used to control the fans, however PWM3 is used to control the LCD brightness.

The LCD brightness must be a value between 0 (off) and 255 (brightest). Control the LCD brightness using the echo command, providing the brightness value you want. The following example sets the LCD to half brightness:
```
echo "128" > /sys/class/hwmon/hwmon1/pwm3
```

## Contrast
TODO

## Text
TODO