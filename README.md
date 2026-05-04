import RPi.GPIO as GPIO
import time

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)

GPIO.setup(11, GPIO.IN)   # PIR motion sensor input
GPIO.setup(3, GPIO.OUT)   # LED output

while True:
    i = GPIO.input(11)

    if i == 0:   # No motion detected
        print("No Person detected", i)
        GPIO.output(3, 0)   # LED OFF
        time.sleep(0.1)

    elif i == 1:   # Motion detected
        print("A Person is detected", i)
        GPIO.output(3, 1)   # LED ON
        time.sleep(0.1)
