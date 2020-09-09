---
layout: course-page
title: "Assignment 3"
permalink: /module3/assignment
description: "Prototyping Connected Product - Assignment 3"
assignment-id: 3
assignment-of: id5415-3
introduction: In this assignment, you will wire three sensors to your Raspberry Pi, to sense light, motion and temperature. You will write code to collect this data, visualise it and trigger actions based on basic data processing.
prog_environment: 
design: 
code_management: GPIO library
computational_concepts: I/O, files
---


---

* Do not remove this line (it will not be displayed)
{:toc}

---

In this assignment, we will make our prototype more aware of its context, with the use of sensor data, bucket, and some simple data processing.   

#  Step 1 GPIO and Sensors

Starting on the hardware side, this section will deal with how we can connect sensors to the raspberry pi (and how the sensors themselves work). 

 For starters,  the raspberry pi can interact with peripherals (such as our sensors) through its GPIO (general purpose input/output) pins: 

Here is an overview of these pins:

![](/assets/img/courses/id5415/module3/assignment/1_0_0.png)

Careful, the physical/board pin number is different from the GPIO number (e.g. physical pin 15 is GPIO22). As you use different libraries, be careful which nomenclature you're using.  For now :

* GPIOzero uses GPIO number
* RPi.GPIO can use either one. 

## Task 1.1 Setup sensor connections to raspberry pi

This assignment will use 2 sensors: 

1. **DHT11** - for temperature and humidity data;
2. **LDR** (light dependent resistor) - this will be our light sensor! 



Mind you, you can use any general GPIO pin for these connections,

For  the LDR, you will need the capacitor in your kit, and a few wires, and you can connect the circuit  in the following manner to the  pi (GPIO18):

![](/assets/img/courses/id5415/module3/assignment/1_1_0.png)



Next, for the DHT, you will need a 10kΩ resistor , some wires, and can connect it like so (GPIO  4): 

TODO ---- img for pi connection ----



## Task 1.2 Setup needed libraries and create your script file

Once we have our wiring, we can now start with the code! In your repository, make sure to create a new .py file - say *sensing.py*,  start up your python virtual environment, and make sure the following libraries are installed by typing each of these instructions in your terminal:

```bash
# GPIO library 
pip install RPI.GPIO 

# GPIO zero library for LDR
pip install gpiozero

# major library to interface with circuit python libraries
pip install adafruit-blinka 

# library for our dht sensor
pip install adafruit-circuitpython-dht 

# necessary system dependency
sudo apt-get install libgpiod2

```



Now we can finish by importing all our libraries in the beginning of  our script: 

```python
import board # for our board pins 
# import DHT sensor library
from Adafruit_DHT import DHT11
# import light sensor from GPIO 0  
from gpiozero import LightSensor # class for ldr connection
```



## Task 1.3 Setup your sensor objects in your python script

Each sensor will have a sensor object through which you can collect data/control the sensor specifications. 

Let's create one for each of these: 

* **DHT11**

  ```python
  # suing gpio pin 4 
  dht_sensor = DHT11(board.d4, use_pulseio=False)
  
  ```

  

* **LDR**

  ```python
  # defining our Light sensor object using GPIO 18
  LDR_pin = 18
  LDR_sensor = LightSensor(LDR_pin)
  ```

  

We are now ready to collect some data! We will be using these classes to retrieve  data and visualize it.  



## Task 1.4 Read raw test sensor data in your script



For a simple test of our sensors, we will read them (after we have set our sensor objects). and print them out in our console once. 

To do this, use the python function "print()" 3 times,  together with each of the three following statements for the  measurements: 

* to get the light value (from 0 to 1) , you can use: `LDR_sensor.value`
* to get the relative  humidity ( from 0 to 100%), you can use: `dht_sensor.humidity `
* to get the temperature in ˚C, you can use: `dht_sensor.temperature`

# Step 2 Data Collection & Processing

At this step, we should now have working sensors (and some data)!  Now we need to structure our data collection, 

do some basic processing,  and send this data to Bucket. 



## Task 2.1 Structure data collection into functions

Now we can replace the print statements in task 1.4 with actual functions that we can call to retrieve our data.  

So we need to create 3 new functions, say `update_temperature()`, `update_humidity()`, and `update_light()`

We will also use another control flow structure, - the try-catch statement, so we can handle cases when the reading data fails (yes, it can happen!). We will use update temperature as an example: 

```python
def update_temperature():
	try:
	    temperature = dht_sensor.temperature
	    print(temperature)
	except RuntimeError as error:
	    # DHT Errors can happen fairly often
	    print(error.args[0]) # specify the problem
	    continue # continue program as normal

	except Exception as error:
    	# this means there is a problem with the actual sensor
	    dht_sensor.exit() # close dht sensor, this statement is not needed if you're using the LDR
	    raise error # this will crash the program 
```



Now, we can create a main function  and call this `update_temperature()` function from it. 

We can do the same creation process for the two other sensors (you can keep the except blocks the same in the try statement, but be sure to update for the proper sensor read instruction!)


## Task 2.2 Send your sensor data to Bucket using the DCD SDK

Following the example of the previous assignments, you can now add the SDK libraries (in the beginning of your script, for clarity) you will need : 

```python
import os 

from dcd.entities.thing import Thing # our thing object
from time import sleep # so we can sleep for a set amount of time 

```



### Task 2.2.1 Create thing and property objects

We  can  now create a new thing in Bucket, or use the one already used for the raspberry pi, instantiate it in our python script, and create 3 new properties in it, one for each sensor.  First, let's add our THING_ID variable , and create our thing object in python ( let's do this right between our sensor objects and our update functions, can you guess why we are doing it before our update functions?)

```python
# library imports and sensor object creation
...
THING_ID = 'MY_THING_ID'

# Instantiate a thing with its credential
my_thing = Thing(thing_id=THING_ID, private_key_path="/etc/ssl/certs/" + THING_ID + ".private.pem")

...
# update functions, main ...
```



Then, let's create 3 new properties (each of its own type) 

```python
# Find or create a property to store light 
my_property_ldr = my_thing.find_or_create_property("LDR sensor", "LIGHT")


# Find or create a property to store temperature
my_property_temp = my_thing.find_or_create_property("DHT Temperature", "TEMPERATURE")


# Find or create a property to store humidity 
my_property_humidity = my_thing.find_or_create_property("DHT Humidity", "RELATIVE_HUMIDITY")
```



### Task 2.2.2 Send updated sensor data to Bucket 



We now need to send this data to Bucket. All this data is one dimensional, which means each time you have an update you only have to send 1 data point to Bucket.  for any one dimensional property, remember that you can update its value with this following instruction structure : `my_property.update_values((my_new_value),)	 `

Now, where would be a good place to add this instruction?  The update functions we made previously, of course!  Instead of printing the new value to the console, we can just send it to Bucket (of course you can also do both). 

For example in the LDR update function we can have:

```python
def update_light():
  try:
    lux = LDR_sensor.value # between 0 (dark) and 1 (light)
    lux = lux*100 # dummy calibration processing, we do not get actual lux values here
    my_property_ldr.update_values((lux,))	
  except RuntimeError as error:
	  print(error.args[0])
	  continue
  except Exception as error:
	  raise error

```



You can see that in this case, we've done a bit of dummy processing to our raw data. We took the basic value (0 to 1). and converted it into a new value, before sending it. This calibration into "fake lux"(light measurement unit) is not right however, as this should be done by calibrating it to your actual environment.  One simple example you could do, would be to convert celsius temperatures to Fahrenheit following its formula in update_temperature()! 



You should now be able to see your data in Grafana!  We will do one last thing - make our script update our values every 2 seconds, forever! This is simple, we can create an infinite while loop inside our main function, call our update functions in series, and wait for 2 seconds with sleep: 

```python
while True:
  update_temperature()
  update_humidity()
  update_light()
  sleep(2)
```



# Step 3 Events and Actions

Oof, almost done now! Now we have a periodic stream of data - a time-series (3 in fact).  The light pre-processing we have done (e.g. fake lux) happens whenever we get a data point, but we want to be able to trigger actions given certain conditions. 

Let's define a simple event and action pair: let's say we want to detect when the room lights where our pi is in get turned on/off.   

* For our action, let's just print to the console "Light Switch has been flipped" 

* For our event,  we need to set a particular threshold for our LDR_sensor value (between 0 and 100)  and trigger this action.

So what do we have to do?  Everytime we get a new time value, we need to see if its value has crossed the threshold.

Lets make a "is_light_on()" function that we call after we get our new light value (to check our threshold, you should make a global variable ( maybe after the properties creation) to hold the previous light value:

```python
def is_light_on(new_value,threshold = 10): 
  # our threshold by default is 10 but you may need to adjust this
  global prev_value # you need this to 
  if(new_value > threshold  and  prev_value < threshold): # we crossed threshold
    print("Light switch has been flipped on")
    
  prev_value = new_value # updating our previous value at the end
```



Note that you can call is_light_on like so: `is_light_on(lux)`, because the threshold by default is 10. if you want to specify it, you can do so as well: `is_light_on(lux, new_threshold)`. 

From this,  can you create a function to trigger when its off?  Can you merge these two functions in one?  

With this, you're free to explore more/different events (detect when a cupboard is open, change bulb brightness according to temperature, etc), and different trigger actions! 





