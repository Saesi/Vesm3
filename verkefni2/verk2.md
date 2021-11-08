##2 Blikandi LED með fade
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
