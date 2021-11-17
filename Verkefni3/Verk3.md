# 3.2
## read
``` python
import RPi.GPIO as GPIO
from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()

try:
        id, text = reader.read()
        print(id)
        print(text)
finally:
        GPIO.cleanup()
```
## Write
``` python
import RPi.GPIO as GPIO
from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()

try:
        text = input('New data:')
        print("Now place your tag to write")
        reader.write(text)
        print("Written")
finally:
        GPIO.cleanup()
```

# 3.3 RFID og LEDs
``` python
import RPi.GPIO as GPIO
from gpiozero import LED
from mfrc522 import SimpleMFRC522
import signal
from time import sleep

reader = SimpleMFRC522()
led1 = LED(18)
led2 = LED(13)

on = True
while on:
    try:
        id, text = reader.read()
        print(id)
        print(text)
        led1.on()
        led2.on()
        sleep(2)
        led1.off()
        led2.off()
    except:
        on = False
        #print("Forrit hefur lokið")
    finally:
        GPIO.cleanup()
```
