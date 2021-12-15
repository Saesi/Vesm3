## 7.1

``` python
from Adafruit_IO import *
aio = Client("Saesi04", "aio_vvoY24bkSn2ckpypEPwBcViasC2w")

num = 42
aio.send("Verk7", num)
```

## 7.2
``` python
from gpiozero import Button
from Adafruit_IO import *
button = Button(18)
aio = Client("Saesi04", "aio_vvoY24bkSn2ckpypEPwBcViasC2w")

def on():
    aio.send("Button", "on")

button.when_pressed = on
```

## 7.3
``` python
import time

# import Adafruit Blinka
import digitalio
import board

# import Adafruit IO REST client.
from Adafruit_IO import Client, Feed, RequestError

# Set to your Adafruit IO key.
# Remember, your key is a secret,
# so make sure not to publish it when you publish this code!
ADAFRUIT_IO_KEY = 'aio_ktIj72cv0P7tBDK2PWNZT1aWByPM'

# Set to your Adafruit IO username.
# (go to https://accounts.adafruit.com to find your username)
ADAFRUIT_IO_USERNAME = 'Saesi04'

# Create an instance of the REST client.
aio = Client(ADAFRUIT_IO_USERNAME, ADAFRUIT_IO_KEY)

try: # if we have a 'digital' feed
    digital = aio.feeds('digital')
except RequestError: # create a digital feed
    feed = Feed(name="digital")
    digital = aio.create_feed(feed)

# led set up
led = digitalio.DigitalInOut(board.D5)
led.direction = digitalio.Direction.OUTPUT


while True:
    data = aio.receive(digital.key)
    if int(data.value) == 1:
        print('received <- ON\n')
    elif int(data.value) == 0:
        print('received <- OFF\n')

    # set the LED to the feed value
    led.value = int(data.value)
    # timeout so we dont flood adafruit-io with requests
    time.sleep(0.5)
```


## 7.5
Ekki get ég gert þetta því ég er ekki með ESP32
