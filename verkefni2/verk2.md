## 1 Blikandi LED ljós
``` python
from gpiozero import LED
from time import sleep

led = LED(18)

while True:
    led.on()
    sleep(1)
    led.off()
    sleep(1)

```

## 2 Blikandi LED með fade
``` python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
GPIO.setup(18, GPIO.OUT)

pwm = GPIO.PWM(18, 50)
pwm.start(0)
while True:
 for i in range(100):
  pwm.ChangeDutyCycle(i)
  time.sleep(0.01)
 for i in range(100, 0, -1):
  pwm.ChangeDutyCycle(i)
  time.sleep(0.01)
```

## 3 LED með takka
``` python
from gpiozero import Button, LED
from signal import pause

button = Button(18)
led = LED(12)

button.when_pressed = led.on
button.when_released = led.off

pause()
```

## 4 Myndavél
``` python
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.start_preview()
camera.capture('/home/pi/Desktop/image.jpg')
camera.stop_preview()
```

## 5 Myndavél með takka
``` python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime
from signal import pause

button = Button(18)
camera = PiCamera()

def capture():
    timestamp = datetime.now().isoformat()
    camera.capture('/home/pi/%s.jpg' % timestamp)

button.when_pressed = capture

pause()
```
## 6 PIR motion sencor LED
``` python
from gpiozero import MotionSensor, LED
from signal import pause

pir = MotionSensor(25)
led = LED(18)

pir.when_motion = led.on
pir.when_no_motion = led.off

pause()
```
