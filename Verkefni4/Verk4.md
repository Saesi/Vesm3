# 4.1
### Raspberry Kóði Hluti 1
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

### Arduino Kóði hluti 1
``` C+
void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println("Hello from Arduino!");
  delay(1000);
}
```

### Raspberry Kóði Hluti 2
``` python
import serial
import time

if __name__ == '__main__':
    ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
    ser.reset_input_buffer()

    while True:
        ser.write(b"Hello from Raspberry Pi!\n")
        line = ser.readline().decode('utf-8').rstrip()
        print(line)
        time.sleep(1)
```

### Arduino Kóði Hluti 2
``` C+
void setup() {
  Serial.begin(9600);
}

void loop() {
  if (Serial.available() > 0) {
    String data = Serial.readStringUntil('\n');
    Serial.print("You sent me: ");
    Serial.println(data);
  }
}
```

# 4.2
### Raspberry pi Kóði
``` python
#!/usr/bin/env python3
import serial
from gpiozero import LED
import time

led = LED(18)

if __name__ == '__main__':
    ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
    ser.reset_input_buffer()

    while True:
        if ser.in_waiting > 0:
            line = ser.readline().decode('utf-8').rstrip()
            print(line)
            time.sleep(4)
            led.on()
            time.sleep(2)
            led.off()
            time.sleep(4)
```

# 4.3
### Raspberry pi Kóði
``` python
import serial
import random

if __name__ == '__main__':
    ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
    ser.reset_input_buffer()

    while True:
        number = ser.read()
        if number != b'':
            if int.from_bytes(number, byteorder='big') == 18:
                led_number = random.randint(1,4)
                print("Button has been pressed.")
                print("Sending number " + str(led_number) + " to Arduino.")
                ser.write(str(led_number).encode('utf-8'))
```

### Arduino Kóði
``` C+
import serial
import random

if __name__ == '__main__':
    ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
    ser.reset_input_buffer()

    while True:
        number = ser.read()
        if number != b'':
            if int.from_bytes(number, byteorder='big') == 18:
                led_number = random.randint(1,4)
                print("Button has been pressed.")
                print("Sending number " + str(led_number) + " to Arduino.")
                ser.write(str(led_number).encode('utf-8'))
```
