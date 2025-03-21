# Scaffold-2-Submission
#This is the code that has been used for my project "Toll Time Crisis" which is my submission for ODT Scaffold #2 assignment - Nikhil Sharma

from machine import Pin, PWM, TouchPad
import neopixel
import time


touchPadPin = 12  
NUM_LEDS = 16
PIN = 22
threshold = 400  


touch_pin = TouchPad(Pin(touchPadPin))  
np = neopixel.NeoPixel(Pin(PIN, Pin.OUT), NUM_LEDS)
lightSensor = Pin(14, Pin.IN)
servo = PWM(Pin(4), freq=50)


def set_servo_angle(angle):
    if 0 <= angle <= 180:
        duty = int((angle / 180) * 102 + 26)
        servo.duty(duty)
    else:
        print("Angle out of range")

# Function to flash NeoPixel red
def flash_red():
    for _ in range(5):  
        np.fill((255, 0, 0))  # Red on
        np.write()
        time.sleep(0.2)
        np.fill((0, 0, 0))  # Red off
        np.write()
        time.sleep(0.2)


def test_neopixel():
    sensorValue = lightSensor.value()
    if sensorValue == 1:
        set_servo_angle(50)
        time.sleep(1)
        np.fill((0, 255, 0))  # Green
        np.write()
    elif sensorValue == 0:
        time.sleep(1)
        set_servo_angle(0)
        time.sleep(1)
        np.fill((255, 0, 0))  # Red
        np.write()

# Main loop
while True:
    capacitiveValue = touch_pin.read()
    if capacitiveValue < threshold:  
        flash_red()
        np.fill((255, 0, 0)) 
        np.write()
        set_servo_angle(40)
        time.sleep(0.4)
        set_servo_angle(0)
        time.sleep(0.4)
        set_servo_angle(40)
        time.sleep(0.4)
        set_servo_angle(0)
        time.sleep(0.4)
        np.fill((255, 0, 0))  
        np.write()
        
  test_neopixel()
  time.sleep(0.1)

