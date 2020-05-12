#!/usr/bin/env pybricks-micropython
#brickrun -r -- pybricks-micropython



'''
connect motors to port A
touch sensor - port 1
'''
from pybricks.hubs import EV3Brick
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.parameters import Color, Port
from pybricks.iodevices import AnalogSensor, UARTDevice
from pybricks.ev3devices import (Motor, TouchSensor,
          ColorSensor, UltrasonicSensor, GyroSensor)
# Initialize the EV3
ev3 = EV3Brick()
ev3.speaker.beep()
x=Motor(Port.A)
t=TouchSensor(Port.S1)
import urequests
import utime, ubinascii, ujson, urequests

Key='7157a849-3f05-4436-9936-01c430c24774'
urlBase = "https://pp-1909111719d4.portal.ptc.io/Thingworx/" # Use your ThingWorx URL
headers = {'Accept':'application/json','Content-Type':'application/json','AppKey':Key}
def Put(thing, property, type, value):
     urlValue = urlBase + 'Things/' + thing + '/Properties/*'

     propValue = {property:value}
     try:
          response = urequests.put(urlValue,headers=headers,json=propValue)
          print(response.text)
          response.close()
     except:
          print('error with Put')
Put('Ev3Data', 'setflag', 'STRING', "reset")
ev3.speaker.say("Press Touch Sensor to Begin")
while True:
    if t.pressed():
        ev3.speaker.say("start measurement")
        wait(1)
        break
    wait(100)


x.reset_angle(0)
ev3.speaker.say("start side one")
while True:
    width=x.angle()*7/360
    if t.pressed():
        ev3.screen.clear()
        ev3.screen.print(" side one" , end='\n')
        ev3.screen.print(" UPLOADING!!!")
        ev3.speaker.say("side one uploading")
        
        Put('Ev3Data', 'x2', 'NUMBER', width)
        ev3.screen.clear()
        break
    wait(100)

x.reset_angle(0)
ev3.speaker.say("start side two")
while True:
    length=x.angle()*7/360
    if t.pressed():
        ev3.screen.clear()
        ev3.screen.print(" side two ",end= '\n')
        ev3.screen.print(" UPLOADING!!!")
        ev3.speaker.say("side two uploading")
        Put('Ev3Data', 'y2', 'NUMBER', length)
        ev3.screen.clear()
        break
    wait(100)

x.reset_angle(0)
ev3.speaker.say("start side three")
while True:
    height=x.angle()*7/360
    if t.pressed():
        ev3.screen.clear()
        ev3.screen.print(" side three ", end='\n')
        ev3.screen.print(" UPLOADING!!!")
        ev3.speaker.say("side three uploading")
        Put('Ev3Data', 'extrude', 'NUMBER', height)
        Put('Ev3Data', 'setflag', 'STRING', "set")    
        ev3.screen.clear()
        break
    wait(100)

