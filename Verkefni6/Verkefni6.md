## 6.2 Takki

´´´ python

from Adafruit_IO import *
import RPi.GPIO as GPIO
from gpiozero import Button

button = Button(18)

ADAFRUIT_IO_KEY = 'aio_tnjy83TmeTOZ64tcIKTsUynbUUBR'
ADAFRUIT_IO_USERNAME = 'Saesi04'

aio = Client(ADAFRUIT_IO_USERNAME, ADAFRUIT_IO_KEY)

while True: # Run forever
    if button == GPIO.HIGH:
        msg = "Button was pushed"
        print("Button was pushed!")
        aio.send("Button",msg)

´´´
