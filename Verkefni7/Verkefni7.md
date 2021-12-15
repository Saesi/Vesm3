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
