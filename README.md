# Bright-or-Dark-Project-with-RaspberryPi3

from twython import *                                                      #After install the code from site, import to our main program
OAUTH_TOKEN = "998223469732139008-g0o0RaE1yisHULWyWYRpMrhkOtIGfm4"         
OAUTH_TOKEN_SECRET = "gmCnefshYhfB7nDCQGJdeAEYLvpZhNToa7pTAiK5Zcz8c"          
APP_KEY = "YFfPXg6vhw9KTPQdKQ7CxAq06"
APP_SECRET = "qMUYiRoESAb8INDzmyoraJ5XA40RQMT8tXsTXae9CLKiv7LKYZ"
twitter = Twython(APP_KEY, APP_SECRET, OAUTH_TOKEN, OAUTH_TOKEN_SECRET)

import RPi.GPIO as GPIO                                                   #They are used terms in code and
import time

GPIO.setwarnings(False)                           
GPIO.setmode(GPIO.BCM)
 
ldr_pin = 3                                                               #It demonstrates LDR is connected which pins of Raspberry Pi
x=0                                                                       #aim of x is add to time end of tweet,aim of counter is average to last 5 data
counter=0

def RCtime (RCpin):
 reading = 0
 GPIO.setup(RCpin, GPIO.OUT)
 GPIO.output(RCpin, GPIO.LOW)
 time.sleep(12)
 
 GPIO.setup(RCpin, GPIO.IN)                  
 while (GPIO.input(RCpin) == GPIO.LOW):
	reading += 1                                                      #Calculation to light density
 return reading
 
while True:
    LDRReading = RCtime(3)
    x=x+LDRReading
    if (counter==5):
        x=x/5
        if (x <= 100):                                               #Decide to light density and tweet to brightness of environment
            my_message=('Is it bright?(%g)' % x)
            twitter.update_status(status=my_message)
            counter=0
            x=0
            time.sleep(60)
        elif (x <=200):
            my_message=('Normal light :)(%g)' % x)
            twitter.update_status(status=my_message)
            counter=0
            x=0
            time.sleep(60)
        else:
            my_message=('Is it dark?(%g)' % x)
            twitter.update_status(status=my_message)
            counter=0
            x=0
            time.sleep(60)
    counter=counter+1                                                #Power cuts during 5 seconds. After five seconds program runs again to be stopped. 
