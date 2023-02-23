
# MQ Sensors for dummies


 **Background**

I wanted to play around with AQI and home assistant, but I didn't want to do coding unless I really, really had to.
I found out that everybody is using these MQ* sensors and I thought I'd give it a go.
Unsurprisingly, I hit a wall pretty quickly and this is my story on how I got around it.

 **Devices**

- ESP32 (I have an AZ-Delivery ESP32 WROOM 32)
- MQ 2 through 9 Sensors + MQ 135 (Bought as a kit on Amazon)
- ADC (analog to digital converter)
- Raspberry Pi4
- Bonus* 
-- Humidity and Temperature sensors 
-- PM Sensor
-- CO2 Sensor

 **Software**

TASMOTA Sensors
Home Assistant
Node-Red plugin for HA
Influx dB + Grafana (for pretty graphs)

## The Expectation
Get PPM (parts per million) readings for different gases in the atmosphere and plot them in Grafana charts and maybe use them for automation (HA would notify me to open a window or run the air purifier if the air outside is worse than inside).

## The Realisation
These MQ sensors don't really return a PPM value, but voltage (and that's also dependant on your input voltage).
AAAAND it's also dependant on the resistor value on the board. 
Ok then, we can do a bit of math if we must, but the function is not linear and there's some interpolation to be had with the different gases and each sensor and gas is different.
And on top of all that, the datasheets don't give you a table or an example or some real world values and it's all written in Chinglish, so good luck making any sense of the highly technical but not at all detailed datasheets.
They just give you two graphs that are vaguely related to one another and, most annoyingly, the graphs were of a very questionable resolution.
But wait, there's more...
YOU HAVE TO CALIBRATE THEM BEFORE USAGE!
And the ESP32 / Arduino / RaspberryPi don't have enough analog inputs and have poor resolution.

## The getting over it

So, since I have already wasted my time and money, let's try to actually use them.
I started trolling the internet and found some valuable information:
1. There are datasheets with better resolutions - one problem solved!
2. There are some guys on the web that have successfully implemented these sensors and were kind enough to make some videos and articles - That was invaluable to writing this.
3. They don't always agree with each other and most of them just do quick hacks, on arduinos, but no integration into something more complex.
4. Some of them either use just the digital output and make an LED beep ;) while others assume  you're a C / arduino / electronics guru and just skim through a lot of stuff like it's common knowledge.
5. There are two very cool, very free, and online pieces of software that have helped me to make sense of the damned graphs.

Now all this has gotten me to this point, where I will assemble all the info a make an idiot's guide (a guide for noobs like me) for these MQ things, because time is money and hopefully someone else will be a bit richer because of me.

## The guide
 I will not bore you with the details from the datasheets and the ABSOLUTELY AWFUL graphs in there.
 Here's some links to the sources I used, but if you want to use the stuff as "out of the box" as possible, you don't even need those.
Suffice to say, I've seen plenty 'nuff of them. So, I made my own and here they are

TL;DR version
Hardware installation
Breadboard with all the 9 sensors and 3 ADCs linked to ground and 5v from the ESP32
The A0 (or analog output) of each of the sensors connected to an analog input of one of the 3 ADC boards
The ADC boards connected together and to the I2C of the ESP32 (SDA and CLK pins). Each jumpered differently so they can have their own address. 
ESP32 connected to a USB power source.
Place the whole thing in fresh air (but not IN the air current). Next to an open window facing a park, in the late spring, an hour or two after a rain shower ;)  
Optional, but recommended: keep the whole contraption on for 48 hours, the first time you build it, before you calibrate. (I noticed a significant difference in readings before and after the "burn-in" period
Optional (for use in configuration) make a note of the resistance value of the one on top, to the left of the small capacitor. Should be different for each sensor.

Software configuration (code and blueprints and all other stuff in the repo)
Install TASMOTA Sensors on the ESP32
Configure it to work with your wifi and MQTT server 
Optional, (but recommended) configure the logging delay to 1s so not to wait 5 minutes between readings) otherwise, the calibration would take a LOOOOOOT of time.
Configure two GPIO pins that take I2C inputs as I2C SDA and  SCL
Get the mqtt messages in node-red and link them to the calibrate function
Optional: Check to see the resistance values in the two functions are the same as the ones on your boards. If not, change accordingly.
Calibrate (wait until you have the Ro values in the debug window) while keeping the sensors in clean air.
Copy the output into the startup of the transform function.
Re-optional, reset your TASMOTA logging to 30s or 1m so as not to fill up the DB with unneeded data
link the output of the transform function to some HA sensor nodes and configure each to extract the info you want.
 add the sensors to your Home assistant dashboards and / or Grafana and customise to your liking.

The long version


> Written with [StackEdit](https://stackedit.io/).
