## Verkefnalýsing

Ég er með LED strips á bak við borð mitt sem vinnur með arduino. Ég var að detta í hug að láta veðrið stjórna hvers konar ljós(array) væri í ferli.
Hægt væri að stjórna birtuni með ljósskynjara. Með takka væri hægt að hafa jólastillingu. Hægt væri að sjá birtu stigið(low, med og high) rgbLED.

## vesen
Ljósnemi minn er ekki að virkar ekki lamennilega svo það er byrjar að blikka

## Þann kóði sem ég er kominn með:
### Raspberry pi:
``` python
import serial
import RPi.GPIO as GPIO
import time
# import Adafruit IO
from Adafruit_IO import Client
import random

# Set to your Adafruit IO key.
ADAFRUIT_IO_KEY = 'aio_tnjy83TmeTOZ64tcIKTsUynbUU'

# Set to your Adafruit IO username.
ADAFRUIT_IO_USERNAME = 'Saesi04'

# Create an instance of the REST client.
aio = Client(ADAFRUIT_IO_USERNAME, ADAFRUIT_IO_KEY)

ser=serial.Serial("/dev/ttyAMA0",9600)
#ser.baudrate=9600

#Því að adafruit virkar ekki þá er þetta sýning á því sem á að gerast
#weather = aio.receive("weather-status")
#print(weather)
snow = "Snow"
cloud = "Cloudy"
rain = "Rainy"
clear = "Clear"

ser.write("Snow")

while True:
    x = random.randint(1, 4)
    if x == 1:
        ser.write(b'Snow')
        print("snow")
    elif x == 2:
        ser.write(b'Cloudy')
        print("cloud")
    elif x == 3:
        ser.write(b'Rain')
        print("rain")
    else:
        ser.write(b'clear')
        print("clear")
    read_ser=ser.readline()
    msg = read_ser.decode('utf-8')
    print(msg.strip())
    time.sleep(1)
    #aio.send("lightvalue", msg)
```

