---
layout: course-page
title: 'Sensor Data Collection'
permalink: /draft/module3/labxp
description: 'Prototyping Connected Products - Lab Experiment 3'
labxp-of: id5415-3
introduction: In this Lab Experiment,
technique:
metrics:
report:
---

---

- Do not remove this line (it will not be displayed)
  {:toc}

---

From assignment 2 and 3, we have now handled code that both gets data from sensors, and also controls the light bulb.

Here is a suggested distribution of tasks among teammates.

![Task Distribution](/assets/img/courses/id5415/module3/labxp3/labxp3-tasks.svg)

In these 4 steps, we will implement the functionality of controlling the lightbulb using inputs from the light sensor.

# Step 1 Housekeep your Code!

In programming, the good programmer is not just the one who can code, but also the one who can best improvise and reuse previous code.

## Task 1.1 Refactor the Code Controlling the Light

Transform the code of in `light.py` to make a class Lightbulb. This class would build `Lightbulb` object with an IP address, a thing id and a path to the private key.

A "shell" of this would look as follows:

```python
class Lightbulb:
   def __init__(self, ip_address, thing_id, private_key_path):
      # Create an attribute bulb of type SmartBulb

      # Establish the the connection with the lightbulb with update()

      # Create an attribute thing_bulb of type Thing, with the thing_id and private_key_path

      # Create an attribute prop_status, result of find_or_create_property()

   def blink(self, num_iterations=10, blink_duration=1):
      # For num_iteration
         # Use self.bulb to turn on the light
         # Transform the result into a list
         # Use self.prop_status to send the new value to Bucket
         # sleep
         # Use self.bulb to turn off the light
         # Transform the result into a list
         # Use self.prop_status to send the new value to Bucket
         # sleep
```

**Note** that the functions `bulb_result_to_list()` and `store_csv_data()` do not have to be part of the class `Lightbulb`, so you can keep it as it is in the file .

Once you have the constructor and the blink method, you can add the other methods that you have developed such as `pulse()` , `morse()` , `frequency()` .

> **Report** On GitHub, in your lab experiment report, report your process of transforming your initial code into a class. What are the pros and cons of this Object-oriented approach?

## Task 1.2 Clean the Sensor Data

We left the data untouched at the end of the previous assignment. Let's make sure that the data is coherent in each of the three methods `update_temperature()` , `update_humidity()` and `update_light()` .

Here is what you should consider:

- do not update the value if it is out of realistic range; e.g if the value is `None`
- do not update the value if it is the same as the previous value;


Then, we need some standard units. The temperature from sensor already comes in Celsius degrees out of the box, and the humidity is a ratio between 0 and 100%.

However, the standard unit for measuring the light is lux. You can read it about lux over here ![Lux Description](https://learn.adafruit.com/photocells/measuring-light).

For a typical LDR, its resistance value will vary according to the lux around it:
![Image of resistance vs illumination](/assets/img/courses/id5415/module3/labxp3/light_graph.gif)

Here we have put a rough formula that relates the resistance of an LDR similar to the one we use, to a lux value:
<img src="https://render.githubusercontent.com/render/math?math=LUX = (1.25 * 10^7) * R^{-1.4059}">

But hey, the values you get from the LightSensor class are from 0(dark) to 1(light)?! 
In actuality, this value is related linearly with the [resistance of the LDR](https://learn.adafruit.com/photocells/arduino-code#bonus-reading-photocells-without-analog-pins-275213-14)

So we need a function  (f(x) = ax+b, a line!) that takes values(x) from 0 to 1, and gives out LDR resistances(f(x)) that make sense! Roughly, we can say for an LDR 
of our type will have a value of 120kΩ for very dark environments, and 336Ω for extreme light.  So our function can look something like this: 
<img src="https://render.githubusercontent.com/render/math?math=y = (336 - 120000) * x %2B 120000">

So given these formulas, you can implement an estimation of the lux value! 
> **Extra side bonus**  Your phone flashlight has a specific lux value, typically around 50 lux. this corresponds to a LDR resistance of ~6.9kΩ, if you use the first lux formula.  If you want to, you could calibrate your curve more to your particular LDR, by adjusting the a parameter in your second formula! For a flashlight of 50 lux, you will get a value x, and a = (6900 - 120000)/x


> **Report** On GitHub, in your lab experiment report, report your process of implementing the data cleaning why you did it this way.

## Task 2.2 Make a Service

So far, we have to login to the Raspberry Pi and start the Python script to collect data and control the lightbulb. To automate this process we need to define a `service` which will automatically start our Python script once the Raspberry Pi is has started.

To do that, first you need to ssh to the pi.

```bash
ssh username@hostname
```

Then start by creating a service file from your Pi's terminal using following three commands:

Create a service file "MY_EXAMPLE" in system directory:

```bash
sudo touch /etc/systemd/system/MY_EXAMPLE.service
```

Give this newly created service file permission to read & write by current logged in user in pi (you)

```bash
sudo chmod 644 /etc/systemd/system/MY_EXAMPLE.service
```

Now open this file in `nano` command line editor:

```bash
sudo nano /etc/systemd/system/MY_EXAMPLE.service
```

Paste the following details in the file and save it. Replace the `ABSOLUTE_PATH/YOUR_SCRIPT` with the location of you light script. And then save it by pressing `CTRL+X`, followed by `y` in prompt to save the file.

```shell
[Unit]
Description=My description
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 ABSOLUTE_PATH/YOUR_PYTHON_SCRIPT.py

StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
User=root
Group=root
[Install]
WantedBy=multi-user.target
```

**NOTE** You must always give the absolute path of your `YOUR_PYTHON_SCRIPT.py` script. You can get that by running the "pwd" in your command line in the file's location, and then appending the "/FILE_NAME" to the response.

Now you can attempt to start the service with the command:

```bash
sudo systemctl start MY_EXAMPLE.service
```

Use status to make sure the service started with no hiccups/errors: If there is an error, you will see in red "script_name running failed". In that case you need to debug the issue with the given error description.

```bash
sudo systemctl status MY_EXAMPLE.service
```

Afterwards stop it, make sure it was stopped properly, and then configure the service to start automatically at boot:

```bash
# Stop service
sudo systemctl stop MY_EXAMPLE.service

# configure automatic start at boot
sudo systemctl enable MY_EXAMPLE.service
```

now restart the pi:

```bash
sudo reboot now
```

If have configured the scripts properly, you will see that once the pi has restarted, it will automatically run your scripts.

> **Report** On GitHub, in your lab experiment report, report your process of implementing this service. What is the purpose? How does it work?

# Step 2 Connect the Sensors to the Lightbulb

## Task 2.1 Pull it together

Merge the code.

Bring it together in the `main.py` :

We need the environment variable of the lightbulb and the Lightbulb class itself

```python
import asyncio
from light import Lightbulb
from dotenv import load_dotenv
import os
load_dotenv()

LIGHTBULB_IP_ADDRESS = os.getenv("LIGHTBULB_IP_ADDRESS", None)
LIGHTBULB_THING_ID = os.getenv("LUGHTBULB_THING_ID", None)
LIGHTBULB_PRIVATE_KEY_PATH = os.getenv("LIGHTBULB_PRIVATE_KEY_PATH", None)
```

Declare the lightbulb as global variable

```python
lightbulb = None
```

Instantiate the lightbulb object in the `main()` function.

```python
lightbulb = Lightbulb(LIGHTBULB_IP_ADDRESS, LIGHTBULB_IP_ADDRESS, LIGHTBULB_PRIVATE_KEY_PATH)
```

Finally, call `main()` in an asynchronous fashion and add the keyword `async` in front of the function.

```python
asyncio.run(main())
```

Finally, trigger the lightbulb method in the `action()` function.

## Task 2.2 Test on the Raspberry Pi

TODO describe each step

Pull the code on the Raspberry Pi

We need to copy the private key of the lightbulb on the Raspberry Pi.

We need to edit the .env with nano and type in the above 3 variables

Execute outside the service

Execute with the service

> **Report** On GitHub, in your lab experiment report, use an architecture diagram to map the component of your system.

# Step 3 Trigger Events

In addition to generating data, our `SensorDataCollector` class could automatically generate events, ready to use.

These events can also be sent to Bucket through a property of type `TEXT`, for example the constructor of this `SensorDataCollector` class could include:

```python
self.event_x_property = self.rpi_thing.find_or_create_property(
            "Event x", "TEXT")
```

To emit an event

```python
def emit_event(self, event_type, value, property):
   if self.event_handler != None:
      self.event_handler({
            'type': event_type,
            'value': value
      })
   property.up
```

## Task 3.1 Built-in light Threshold Event

Besides the functions we've defined, you can adjust the threshold (and add trigger functions) directly in the [LightSensor class](https://gpiozero.readthedocs.io/en/stable/api_input.html#lightsensor-ldr):

1. In your physical setup, adjust the threshold value (passed when creating the class), so that it detects internally when you pass your hand over the LDR.

2. Create two new functions "hand_detected" and "no_hand_detected" , and set your LightSensor object "when_dark" and "when_light" properties to the proper function,right after creating it.  eg: `LDR_sensor.when_dark = MyFunctionName" `
3. In these new functions, print a corresponding statement to your console, e.g. "Hand detected"

4. Besides the print statement, turn on the kasa lightbulb when you have detected the hand

## Task 3.2 Enter/Leave Condition Event
In the class `SensorDataCollector`, develop a method (function) that

- receives the sensor data
- define thresholds ( ranges for the value of your data, e.g. - 20˚ to 25˚ is cozy - that characterise some conditions (cold, cosy, e.g.  *tip - use if-elif structure!*)
- checks the new data against the threshold(e.g if temp<20: print(cosy) bulb_brightness= high)
- emit an event if the conditions have changed

## Task 3.3 Trend Event

In the class `SensorDataCollector`, develop a method(function) that

- receives the sensor data (refer to assignment 3)
- keeps a record of the data points over the past minute ( note your sensor collector class takes new events every X seconds - how many datapoints would make up a minute?)
- evaluate some trends (values in record are on average decreasing, increasing or constant...)
- emit an event if the trend changed ( eg, on average temperature values are increasing, emit a  "it's getting hot")

# Step 4 Control based on events

In this final step, you control the lightbulb based on events triggered by the data collection.

- in SensorDataCollector, like the handler for the raw values, add a handler setEventHandler() to listen to events
- call the three event methods
- in main.py, define event_action(), the function that is triggered when there is a new event

```python
async def main():
   # ...
   collector.set_event_handler(event_action)
```

```python
def event_action(event):
    print('ready for action')
    if event["type"] == "x":
        # control the light
    elif event["type"] == "y":
        # control the light
    elif event["type"] == "z":
        # control the light
```

> **Report** On GitHub, in your lab experiment report, use a flow chart to map the flow from sensor data collection to lightbulb control.
