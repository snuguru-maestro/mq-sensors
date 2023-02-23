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
These MQ sensors don't really return a PPM value, but voltage (and that's also dependant on your input voltage)
AAAAND it's also dependant on the resistor value on the board
Ok, then, we can do a bit of math if we must, but the function is not linear and there's some interpolation to be had with the different gases and each sensor and gas is different.
And on top of all that, the datasheets don't give you a table or an example or some real world values.
They just give you two graphs that are vaguely related to one another and, most annoyingly, the graph was of a very questionable resolution.
But wait, there's more... It's all written in Chinglish so good luck making any sense of that...

## The beginning

I started trolling the internet and found some valuable information:
1. There are different datasheets with different resolutions - one problem solved!
2. There are some guys on the web that have successfully implemented these sensors and were kind enough to make some videos
3. They don't always agree with each other and most of them just do quick hacks, but no integration.
4. There are two very cool very free pieces of software that have helped me to make sense of the damned graphs.

Now all this has gotten me to this point, where I will assemble all the info a make an idiot's guide (a guide for noobs like me) for these MQ things.

## The guide
 I will not bore you with the details from the datasheets and the ABSOLUTELY AWFUL graphs in there.
 Here's some links to my sources, but if you want to use the stuff as "out of the box" as possible, you don't even need those.
Suffice to say, I've seen plenty 'nuff of them, so, I made my own and here they are




> Written with [StackEdit](https://stackedit.io/).
