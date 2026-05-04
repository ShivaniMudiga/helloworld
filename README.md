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

---------------------------------------------------------
# Libraries
import RPi.GPIO as GPIO
import time

# GPIO Mode (BCM numbering)
GPIO.setmode(GPIO.BCM)

# Define GPIO pins
GPIO_TRIGGER = 18
GPIO_ECHO = 24

# Set GPIO direction
GPIO.setup(GPIO_TRIGGER, GPIO.OUT)
GPIO.setup(GPIO_ECHO, GPIO.IN)


def distance():
    # Send trigger pulse
    GPIO.output(GPIO_TRIGGER, True)
    time.sleep(0.00001)  # 10 microseconds
    GPIO.output(GPIO_TRIGGER, False)

    # Initialize start and stop time
    start_time = time.time()
    stop_time = time.time()

    # Save start time
    while GPIO.input(GPIO_ECHO) == 0:
        start_time = time.time()

    # Save arrival time
    while GPIO.input(GPIO_ECHO) == 1:
        stop_time = time.time()

    # Time difference
    time_elapsed = stop_time - start_time

    # Calculate distance (speed of sound = 34300 cm/s)
    dist = (time_elapsed * 34300) / 2

    return dist


# Main program
if __name__ == '__main__':
    try:
        while True:
            dist = distance()
            print("Measured Distance = %.1f cm" % dist)
            time.sleep(1)

    except KeyboardInterrupt:
        print("Measurement stopped by User")
        GPIO.cleanup()


---------------------------------------------------------

from picamera import PiCamera
from time import sleep

camera = PiCamera()
camera.rotation = 180

# Start camera preview
camera.start_preview()
sleep(5)  # Allow camera to warm up

# Capture image
camera.capture('/home/pi/Desktop/image.jpg')

# Stop preview
camera.stop_preview()

from picamera import PiCamera, Color
from time import sleep

camera = PiCamera()
camera.rotation = 180

camera.start_preview()

# Apply effects
camera.image_effect = 'watercolor'

# Add text styling
camera.annotate_background = Color('yellow')
camera.annotate_foreground = Color('blue')
camera.annotate_text_size = 80
camera.annotate_text = "Hello KMIT"

# Capture 5 images
for i in range(5):
    sleep(5)
    camera.capture(f'/home/pi/Desktop/Rasp{i}.jpg')

camera.stop_preview()

from picamera import PiCamera, Color
from time import sleep

camera = PiCamera()
camera.rotation = 180

camera.start_preview()

# Add text overlay
camera.annotate_background = Color('yellow')
camera.annotate_foreground = Color('blue')
camera.annotate_text_size = 80
camera.annotate_text = "KMIT IT"

# Start recording
camera.start_recording('/home/pi/Desktop/video.h264')
sleep(5)

# Stop recording
camera.stop_recording()
camera.stop_preview()
