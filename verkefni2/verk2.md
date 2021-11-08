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

## 7 PIR motion sencor og myndavél
``` python
import RPi.GPIO as GPIO
import time
import datetime
import os
import smtplib
from email import encoders
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from gpiozero import MotionSensor
from picamera import PiCamera
from datetime import datetime
from signal import pause

def capture():
    timestamp = datetime.now().isoformat()
    camera.capture('/home/pi/%s.jpg' % timestamp)

pir = MotionSensor(18)
camera = PiCamera()
GPIO.setmode(GPIO.BCM)

pir.when_motion = capture

GPIO.setup(23, GPIO.IN) #PIR
GPIO.setup(24, GPIO.OUT) #BUzzer

'''
ts = time.time()
st = datetime.datetime.fromtimestamp(ts).strftime('%Y-%m-%d %H:%M:%S')
'''



COMMASPACE = ', '

def Send_Email(image):
    sender = '###YOUREMAIL###'
    gmail_password = '###YOURPASSWORD###'
    recipients = ['##YOURRECIPENTEMAIL###']

    # Create the enclosing (outer) message
    outer = MIMEMultipart()
    outer['Subject'] = 'Attachment Test'
    outer['To'] = COMMASPACE.join(recipients)
    outer['From'] = sender
    outer.preamble = 'You will not see this in a MIME-aware mail reader.\n'

    # List of attachments
    attachments = [image]

    # Add the attachments to the message
    for file in attachments:
        try:
            with open(file, 'rb') as fp:
                msg = MIMEBase('application', "octet-stream")
                msg.set_payload(fp.read())
            encoders.encode_base64(msg)
            msg.add_header('Content-Disposition', 'attachment', filename=os.path.basename(file))
            outer.attach(msg)
        except:
            print("Unable to open one of the attachments. Error: ", sys.exc_info()[0])
            raise

    composed = outer.as_string()

    # Send the email
    try:
        with smtplib.SMTP('smtp.gmail.com', 587) as s:
            s.ehlo()
            s.starttls()
            s.ehlo()
            s.login(sender, gmail_password)
            s.sendmail(sender, recipients, composed)
            s.close()
        print("Email sent!")
    except:
        print("Unable to send the email. Error: ", sys.exc_info()[0])
        raise



try:
    time.sleep(2) # to stabilize sensor
    
            
    while True:
        ##Timeloop
        ts = time.time()
        st = datetime.datetime.fromtimestamp(ts).strftime('%Y-%m-%d %H:%M:%S')
        if GPIO.input(23):
            ##If loop
            GPIO.output(24, True)
            time.sleep(0.5) #Buzzer turns on for 0.5 sec
            print("Motion Detected at {}".format(st))
            ##Adds timestamp to image
            camera.capture('image_Time_{}.jpg'.format(st))
            image = ('image_Time_{}.jpg'.format(st))
            Send_Email(image)
            time.sleep(2)
            GPIO.output(24, False)
            time.sleep(5) #to avoid multiple detection

        time.sleep(0.1) #loop delay, should be less than detection delay

except:
    GPIO.cleanup()
pause()
```
