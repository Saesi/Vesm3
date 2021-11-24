#4.1
## Raspberry Kóði
``` python
import serial
from gpiozero import LED

led = LED(18)

if __name__ == '__main__':
    ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
    ser.reset_input_buffer()

    while True:
        if ser.in_waiting > 0:
            line = ser.readline().decode('utf-8').rstrip()
            print(line)
            led.on()
```
