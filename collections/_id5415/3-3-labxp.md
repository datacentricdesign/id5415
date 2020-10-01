---
layout: course-page
title: 'Maintain, Connect and Structure Code'
permalink: /module3/labxp
description: 'Prototyping Connected Products - Lab Experiment 3'
labxp-of: id5415-3
introduction: From assignment 2 and 3, we have now developed the code that gets data from sensors, and also controls the light bulb. In this lab experiment, we will use code refactoring, data cleaning and services to make our program easier to maintain. We will see how to control the lightbulb from the raw sensor data as well as higher-level events.
technique:
metrics:
report:
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---

Here is a suggested distribution of tasks among teammates. Branches mean that these tasks can be done in parallel. In these 4 steps, we will implement the functionality of controlling the lightbulb using inputs from the light sensor.

![Task Distribution](/assets/img/courses/id5415/module3/labxp3/labxp3-tasks.svg)

# Step 1 Housekeep your Code

When prototyping, our code is growing organically. It is typical, as each iteration leads us to adjust our plan organically. However, it is important to regularly zoom out and take a moment to tidy up our code so that it stays maintainable. This process also help to make (part of) our code reusable, avoiding to rewrite similar pieces of code.

## Task 1.1 Refactor the Code Controlling the Light

This first task is about [code refactoring](https://en.wikipedia.org/wiki/Code_refactoring): the process of restructuring code. We want to transform the code in `light.py` to make a class Lightbulb. This class would build `Lightbulb` objects with an IP address, a thing id and a path to the private key. The [skeleton](https://en.wikipedia.org/wiki/Skeleton_(computer_programming)) of this class looks as follows. Copy and paste this skeleton in your file and add the corresponding line of code for each comment.

``` python
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
   
def bulb_result_to_list(bulb_result):

def store_csv_data(values):
```

**Note** that the functions `bulb_result_to_list()` and `store_csv_data()` do not have to be part of the class `Lightbulb` , so you can keep it as it is in the file.

Once you have the constructor and the blink method, you can add the other methods that you have developed such as `pulse()` , `morse()` , `frequency()`.

> **Report** On GitHub, in your lab experiment report, report your process of transforming your initial code into a class. What are the pros and cons of this Object-oriented approach?

## Task 1.2 Clean the Sensor Data

The second housekeeping task is about the data. We left the data untouched at the end of the previous assignment, directly coming out of the sensors. Let's make sure that the data is coherent in each of the three methods `update_temperature()`, `update_humidity()` and `update_light()`.

Here is what you should consider:

* do not update the value if it is out of realistic range. For example, you could check if the value is `None` or Negative;
* do not update the value if it is the same as the previous value; 

Then, we need some standard units. The value of temperature from the DHT sensor already comes in Celsius degrees out-of-the-box. The relative humidity is a ratio, between 0 and 100%, which does not require any intervention. However, the standard unit for measuring the light is lux. You can read more about lux on [this Adafruit tutorial about light measurement](https://learn.adafruit.com/photocells/measuring-light).

For a typical LDR, the resistance value will vary according to the lux around it:

![Image of resistance vs illumination](https://cdn-learn.adafruit.com/assets/assets/000/000/452/original/light_graph.gif?1447975659)

(Source: [Adafruit - Measuring light](https://learn.adafruit.com/photocells/measuring-light))

Here we have a rough formula that relates the resistance of an LDR similar to the one we use, to a lux value:
<img src="https://render.githubusercontent.com/render/math?math=LUX = (1.25 * 10^7) * R^{-1.4059}">

However, the values we get from the `LightSensor` class range from 0 (dark) to 1 (light). This value has a linear relationship with the [resistance of the LDR](https://learn.adafruit.com/photocells/arduino-code#bonus-reading-photocells-without-analog-pins-275213-14)

To map our values, we need a function `f(x) = ax+b` that takes a value `x` from `0` to `1` and gives out the LDR resistance `f(x)`. Roughly, we can say for an LDR
of our type will have a value of `120kΩ` for very dark environments, and `336Ω` for extreme light. Our function can look something like this:

```
y = (336 - 120000) * x + 120000
```

Implement an estimation of lux using this formula and send this value on Bucket instead of the raw sensor value.

> **Extra side bonus** Your phone flashlight has a specific lux value, typically around 50 lux. This corresponds to an LDR resistance of `~6.9kΩ` if you use the first lux formula. If you want to, you could calibrate this curve more specifically to your LDR by adjusting the parameter in the second formula. For a flashlight of 50 lux, you will get a value `x`, and `a = (6900 - 120000)/x`

> **Report** On GitHub, in your lab experiment report, report your process of implementing the data cleaning why you did it this way.

## Task 1.3 Make a Service

The last housekeeping task ensures our code is automatically executed when the Raspberry Pi starts. Until now, we have to log on to the Raspberry Pi and start the Python script to collect data and control the lightbulb. To automate this process, we need to define a `service` which will automatically start our Python script when the Raspberry Pi is starting.

Let's open a Terminal and log on the Raspberry Pi.

``` bash
ssh [username]@[hostname].local
```

Then, we create a service file from your Pi's terminal using the following three commands:

To create a service, we need a service file. This file should be located in the specific directory `/etc/systemd/system`. We use the following command to create a service file `light.service`. In this command, `sudo` is the administration mode as we are manipulating system files and directory. `touch` is the command to create an empty file.

``` bash
sudo touch /etc/systemd/system/light.service
```

Then, give to the current user on the Raspberry Pi (yourself), the permission to read and write in this newly created service file. `chmod`  is the command to set permission on a file ([Examples](https://www.lifewire.com/uses-of-command-chmod-2201064)).

``` bash
sudo chmod 644 /etc/systemd/system/light.service
```

Now open this file in `nano`, the command-line editor:

``` bash
sudo nano /etc/systemd/system/light.service
```

Paste the following details in the file. Replace the `ABSOLUTE_PATH/YOUR_SCRIPT` with the location of you `main.py`. Then, save it by pressing `CTRL+x` , followed by `y` (answering 'yes') when prompted to save the file.

``` shell
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

**NOTE** We must always give the absolute path of your `YOUR_PYTHON_SCRIPT.py` script. We can get that by running the command `pwd` in your command-line in the file's location, and then appending the "/FILE_NAME" to the response. Now you can attempt to start the service with the command:

``` bash
sudo systemctl start light.service
```

Use the `status` parameters to make sure the service started with no hiccups/errors: If there is an error, you will see in red "script_name running failed". In that case, you need to debug the issue with the given error description.

``` bash
sudo systemctl status light.service
```

You can stop the script with the parameter `stop`. Make sure it was stopped properly, and then configure the service to start automatically when the Raspberry Pi is starting with the parameter `enable`.

``` bash
# Stop service
sudo systemctl stop light.service

# configure automatic start at boot
sudo systemctl enable MY_EXAMPLE.service
```

Finally, test your setup by restarting your Raspberry Pi using the following command:

``` bash
sudo reboot now
```

If you configured the script properly, your Python script should start as soon as the Raspberry Pi started.

> **Report** On GitHub, in your lab experiment report, report your process of implementing this service. What is the purpose? How does it work?

# Step 2 Connect the Sensors to the Lightbulb

It is now time to bring the code of all teammates together.

## Task 2.1 Pull it together

First, let's commit, merge and push the latest changes of everyone.

Then, we can connect sensing and lightbulb actions in the `main.py`. We need the environment variable of the lightbulb and the Lightbulb class itself

``` python
import asyncio
from light import Lightbulb
from dotenv import load_dotenv
import os
load_dotenv()

LIGHTBULB_IP_ADDRESS = os.getenv("LIGHTBULB_IP_ADDRESS", None)
LIGHTBULB_THING_ID = os.getenv("LUGHTBULB_THING_ID", None)
LIGHTBULB_PRIVATE_KEY_PATH = os.getenv("LIGHTBULB_PRIVATE_KEY_PATH", None)
```

We declare the lightbulb as `global` variable.

``` python
lightbulb = None
```

We instantiate the lightbulb object in the `main()` function.

``` python
lightbulb = Lightbulb(LIGHTBULB_IP_ADDRESS, LIGHTBULB_IP_ADDRESS, LIGHTBULB_PRIVATE_KEY_PATH)
```

Finally, call `main()` in an asynchronous fashion and add the keyword `async` in front of the function.

``` python
asyncio.run(main())
```

Finally, trigger the lightbulb method in the `action()` function.

As usual, we commit and push our code.

## Task 2.2 Test on the Raspberry Pi

To test our code on the Raspberry Pi, we need to first pull it from GitHub. To do that, we use `ssh` to connect to the Raspberry Pi and navigate to our project directory using the `cd` command. Then, we use the following command to pull the code.

``` bash
git pull # The command will fetch all the new changes into the Raspeberry Pi directory.
```

Because our lightbulb Thing on Bucket is secured with a private key, we need to copy the private key of the lightbulb from our laptop to the Raspberry Pi. We can do that with the `scp` command we used in the first lab experiment. This command is copying files remotely, from our project directory where `private.pem` is stored to the Raspberry Pi directory:

>**Windows** To use the `scp` command in Windows you first need to download & install the SCP client software [SCP Client for Windows x64](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe).

``` bash
scp private.pem [username]@[hostname].local:~/PATH_TO_YOUR_PROJECT_FOLDER/ #It will copy the file to your project directory on Pi
```

As we are not uploading the `.env` file on GitHub, we need to create and edit this file with command-line editor `nano`.

First, in the Raspberry Pi project directory, create `.env` file using the `touch` command:

``` bash
touch .env
```

Then, open the file using `nano`:

``` bash
nano .env
```

Finally, copy the following three lines and add the values for the Thing id of your lightbulb, the path of the `private.pem` , and your bulbs' IP Address:

``` bash
LIGHTBULB_THING_ID=
LIGHTBULB_PRIVATE_KEY_PATH=
LIGHTBULB_IP_ADDRESS=
LOG_LEVEL=INFO
```

Press `CTRL+x` to exit and type `y` when asked to save the file.

We can now execute the `main.py` scripts. It should connect to the bulb and start sending data to Bucket. The control of the lightbulb from the sensor depends on the our function `action()`

You can also make this process as service, so every time the pi starts, it will run the `main.py` scripts automatically.

To do that follow [Task 1.3](#task-13-make-a-service)

> **Report** On GitHub, in your lab experiment report, use an architecture diagram to map the component of your system.

# Step 3 Trigger Events

In addition to collecting data, our `SensorDataCollector` class could automatically generate events such as 'The light is dark', 'The humidity is too high' of 'The temperature is cosy'. Emitting events will simplify the way we trigger actions. Instead of observing each data point to decide whether to turn the light ON, we could directly react to the event 'Light is dark'.

Let's define an event as a Dictionary of 2 values: a type and a value. Using the temperature as an example, we could emit events like these

```python
{ 'type': 'temperature_condition', 'value': 'Cold' }
{ 'type': 'temperature_condition', 'value': 'Cosy' }
{ 'type': 'temperature_condition', 'value': 'Warm' }
```

To receive these events, we follow the same approach as when we receive raw data: we set a handler. To do this, we add an attribute (the variable of a class)

```python
def __init__(self):
   # ...
   self.event_handler = None
```

To set this handler, we define a method (the function of a class) which takes a function as a single parameter set be used as a handler.

```python
def set_event_handler(self, handler):
   self.event_handler = handler
```

Finally, to emit an event, we define the following method:

``` python
def emit_event(self, event_type, value, property):
   """
   Prepare and send an event if a handler is set.

   Args:
      event_type (str): A string defining the type of the event (e.g. 'temperature_condition')
      value (str): A string describing the condition (e.g. 'Cosy')
   """

   # Is there a handler to call
   if self.event_handler != None:
      # Then prepare the event
      event = { 'type': event_type, 'value': value }
      # Call the handler with the event as parameter
      self.event_handler(event)
```

At this stage, we need three elements for each type of event we want to generate. Let's take the example of the temperature condition. In the constructor, we first need to initialise an attribute (the variable of a class) to keep track of the current  condition. The initial value is None as we do not know yet the condition.

```python
def __init__(self):
   # ...
   self.temperature_condition = None
```

Then, we need to define a method that will evaluate the temperature condition.

``` python
def evaluate_temperature(self, event_type, value, property):
   # Is the temperature lower than 19?
      # Is this condition different than before?
         # Emit an event
         # Set this condition as the current condition
```

Finally, we need to call this method each time we collect new data. The `collect()` method looks appropriate for that.

```python
def collect(self):
   # ...
   self.evaluate_temperature()
```

## Task 3.1 Generate humidity_condition events

Add the necessary code to generate humidity_condition events.

## Task 3.2 Generate ligh_condition events

Add the necessary code to generate light_condition events.

## Task 3.3 Generate x_condition events

Add the necessary code to generate a condition of your choice, that relies on the values of several sensors.


> **Report** On GitHub, in your lab experiment report, reflect on this event mechanism and the type of event your connected product can emit.

> **Bonus**: you also send this data to Bucket using the property type `TEXT`. For this, you can use the `find_or_create()` method of Thing and `update_value()` of a property, as we did for the raw values.

# Step 4 Control based on events

In this final step, you control the lightbulb based on events triggered by the data collection.

* in SensorDataCollector, like the handler for the raw values, add a handler `setEventHandler()` to listen to events
* call the methods generating events
* in main.py, define event_action(), the function that is triggered when there is a new event

``` python
async def main():
   # ...
   collector.set_event_handler(event_action)
```

``` python
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
