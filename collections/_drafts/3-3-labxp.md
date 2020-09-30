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

From assignment 2 and 3, we have now developed the code that gets data from sensors, and also controls the light bulb.

In todays lab experiment, we will tr to improvise and simplify those code. E.g create separate class for each functionality use this classes in `main.py`

We will use the light sensor to control the light.

We will also make an automate pi service that will run our python script automatically once the pi is started.

Here is a suggested distribution of tasks among teammates.

![Task Distribution](/assets/img/courses/id5415/module3/labxp3/labxp3-tasks.svg)

In these 4 steps, we will implement the functionality of controlling the lightbulb using inputs from the light sensor.

# Step 1 Housekeep your Code!

In programming, the good programmer is not the one who can just code, but the one who can improvise the code to reuse without not much modification on the core functionality.

## Task 1.1 Refactor the Code Controlling the Light

Transform the code in `light.py` to make a class Lightbulb. This class would build `Lightbulb` object with an IP address, a thing id and a path to the private key.

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
def bulb_result_to_list(bulb_result)....

def store_csv_data(values)....
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

However, the standard unit for measuring the light is lux. You can read more about lux over here [Lux Description](https://learn.adafruit.com/photocells/measuring-light).

For a typical LDR, its resistance value will vary according to the lux around it:
![Image of resistance vs illumination](/assets/img/courses/id5415/module3/labxp3/light_graph.gif)

Here we have put a rough formula that relates the resistance of an LDR similar to the one we use, to a lux value:
$ Lux = (1.25 _ 10^7) _ R_LDR^ -1.4059 $

But hey, the values you get from the LightSensor class are from 0(dark) to 1(light)! In actuality this value is related linearly with the [resistance of the LDR](https://learn.adafruit.com/photocells/arduino-code#bonus-reading-photocells-without-analog-pins-275213-14)

So we need a function (f(x) = ax+b, a line!) that takes values(x) from 0 to 1, and gives out LDR resistances(f(x)) that make sense! Roughly, we can say for an LDR
of our type will have a value of 120kΩ for very dark environments, and 336Ω for extreme light. So our function can look something like this:
$ y = (336 - 120000) * x + 120000 $

So given these formulas, you can implement an estimation of lux!

> **Extra side bonus** Your phone flashlight has a specific lux value, typically around 50 lux. this corresponds to a LDR resistance of ~6.9kΩ, if you use the first lux formula. If you want to, you could calibrate your curve more to your particular LDR, by adjusting the a parameter in your second formula! For a flashlight of 50 lux, you will get a value x, and a = (6900 - 120000)/x

> **Report** On GitHub, in your lab experiment report, report your process of implementing the data cleaning why you did it this way.

## Task 1.3 Make a Service

So far, we have to start login on the Raspberry Pi and start the Python script to collect data and control the lightbulb. To automate this process we need to define a `service` which will automatically start our Python script once the Raspberry Pi is has started.

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

you might consider to commit your code here and push it into the git branch!

## Task 2.2 Test on the Raspberry Pi

To test our ne code that we have implemented on pi, we need to first pull it from our remote git repository (from github server) into the pi.

To do that, ssh to the pi and go to your github project location using `cd` command. and then simply type"

```bash
git pull #The command will fetch all the new changes into the pi directory.
```

Now because our lightbulb thing on the bucket is secured with private key, You cannot access it without providing this private key. So We need to copy the private key of the lightbulb from your laptop project folder to the Raspberry Pi. You can do that simply by running copy command from your project directory, where `private.pem` is stored:

**Mac:**

```bash
scp private.pem [username]@[hostname].local:~/PATH_TO_YOUR_PROJECT_FOLDER/
```

**Windows**

To use the `scp` command in windows you first need to download & install the SCP client software. ![SCP Client for Windows](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe)

once you have installed the scp client, you can got to your project directory where `private.pem` is stored using `cd` command and type:

```bash
scp private.pem [username]@[hostname].local:~/PATH_TO_YOUR_PROJECT_FOLDER/ #It will copy the file to your project directory on Pi
```

As we are not uploading the env file on git hub, We need to create and edit the .env with `nano` command line editor. and type in the above 3 variables

First in pi project folder, create .env file using:

```bash
touch .env
```

then open the file using `nano` editor

```bash
nano .env
```

and copy the following three lines, and replace the thing id by your lightbulb thing id, path of the `private.pem`, and your bulbs' IP Address :

```bash
THING_ID= YOUR_BULB_THING_ID
PRIVATE_KEY_PATH= PATH_TO_PRIVATE_KEY
LIGHTBULB_IP_ADDRESS= BULB_IP_ADDRESS
LOG_LEVEL=INFO
```

Press CTRL+X to sava and type `y` on prompt to save the file.

Now to check if everything works fine: just run the `main.py`scripts and it should connect to the bulb and start sending data to bucket.

You can also make this process as service, so every time the pi starts, it will run the `main.py` scripts automatically.

To do that follow [Task 1.3](#task-13-make-a-service)

> **Report** On GitHub, in your lab experiment report, use an architecture diagram to map the component of your system.

# Step 3 Trigger Events

In addition to generating data, our `SensorDataCollector` class could automatically generate events, ready to use.

These events can also be sent to Bucket through a property of type `TEXT`, for example the constructor of this `SensorDataCollector` class could include:

```python
self.event_x_property = self.rpi_thing.find_or_create_property(
            "Event x", "TEXT")
```

To emit an event from `SensorDataCollector` you can create a function similar:

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

2. Create two new functions "hand_detected" and "no_hand_detected" , and set your LightSensor object "when_dark" and "when_light" properties to the proper function, eg: `LDR_sensor.when_dark = MyFunctionName" `
3. In these new functions, print a corresponding statement to your console, e.g. "Hand detected"

4. Besides the print statement, turn on the kasa lightbulb when you have detected the hand

## Task 3.2 Enter/Leave Condition Event

TODO give hints where necessary

In the class `SensorDataCollector`, develop a method that

- receive the sensor data
- define thresholds that characterise some conditions (cold, cosy, e.g. )
- check the new data against the threshold(e.g if temp<20: print(cosy) bulb_brightness= high)
- emit an event if the condition changed

## Task 3.3 Trend Event

TODO give hints where necessary

In the class `SensorDataCollector`, develop a method that

- receive the sensor data
- keep a record of the data points over the past minute
- evaluate the trend (decreasing, increasing or constant)
- emit an event if the trend changed

# Step 4 Control based on events

In this final step, you control the lightbulb based on events triggered by the data collection.

- in SensorDataCollector, like the handler for the raw values, add a handler setEventHander() to listen to events
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
