---
layout: course-page
title: "Sensor Data Collection"
permalink: /draft/module3/labxp
description: "Prototyping Connected Products - Lab Experiment 3"
labxp-of: id5415-3
introduction: In this Lab Experiment, 
technique:
metrics:
report:
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---

From assignment 2 and 3, we have code to sense data from sensors and control the light bulb.

Here is a suggested distribution of tasks among teammates.

![Task Distribution](/assets/img/courses/id5415/module3/labxp3/labxp3-tasks.svg)

# Step 1 Make Housekeeping on the Code

## Task 1.1 Refactor the Code Controlling the Light

Transform the code of in `light.py` to make a class Lightbulb. This class would build `Lightbulb` object with an IP address, a thing id and a path to the private key. This would look as follows.

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
```

**Note** that the functions `bulb_result_to_list()` and `store_csv_data()` do not have to be part of the class `Lightbulb` .

Once you have the constructor and the blink method, you can add the other methods that you have developed such as `pulse()` , `morse()` , `frequency()` .

> **Report** On GitHub, in your lab experiment report, report your process of transforming your initial code into a class. What are the pros and cons of this Object-oriented approach?

## Task 1.2 Clean the Sensor Data 

We left the data untouched at the end of the previous assignment. Let's make sure that the data is coherent in each of the three methods `update_temperature()` , `update_humidity()` and `update_light()` .

Here is what you should consider:

* do not update the value if it is out of realistic range; 
* do not update the value if it is the same as the previous value; 

Then, we need some standard units. The temperature comes in Celsius degree out of the box and the relative humidity is a ratio. It leaves the light.

TODO offer the mathematic formula for lux conversion

> **Report** On GitHub, in your lab experiment report, report your process of implementing the data cleaning why you did it this way.

## Task 1.3 Make a Service

So far, we have to start login on the Raspberry Pi and start the Python script to collect data and control the lightbulb. To automate this process we need to define a `service` which will automatically start our Python script when the Raspberry Pi is starting.

TODO describe how to run the script as a service (we had a tutorial on this from last year), restart if failing

> **Report** On GitHub, in your lab experiment report, report your process of implementing this service. What is the purpose? How does it work?

# Step 2 Connect the Sensors to the Lightbulb

## Task 2.1 Pull it together

Merge the code.

Bring it together in the `main.py` :

We need the environment variable of the lightbulb and the Lightbulb class itself

``` python
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

``` python
lightbulb = Lightbulb(LIGHTBULB_IP_ADDRESS, LIGHTBULB_IP_ADDRESS, LIGHTBULB_PRIVATE_KEY_PATH)
```

Finally, call `main()` in an asynchronous fashion and add the keyword `async` in front of the function.

``` python
asyncio.run(main())
```

Finally, trigger the lightbulb method in the `action()` function.


## Task 2.2 Test on the Raspberry Pi

Pull the code on the Raspberry Pi

We need to copy the private key of the lightbulb on the Raspberry Pi.

We need to edit the .env with nano and type in the above 3 variables

Execute outside the service

Execute with the service

# Step 3 Trigger Events

In addition to generating data, our SensorDataCollector could automatically generate events, ready to use. These events can also be sent to Bucket through a property of type `TEXT`.

## Task 3.1 Built-in light Threshold Event

Besides the functions we've defined, you can adjust the threshold (and add trigger functions) directly in the [LightSensor  class](https://gpiozero.readthedocs.io/en/stable/api_input.html#lightsensor-ldr):

1. In your physical setup, adjust the threshold value (passed when creating the class), so that it detects internally when you pass your hand over the LDR.

2. Create two new functions "hand_detected" and "no_hand_detected" , and set your LightSensor object "when_dark" and "when_light" properties to the proper function, eg: `LDR_sensor.when_dark = MyFunctionName" `
3. In these new functions, print a corresponding statement to your console, e.g. "Hand detected"

4. Besides the print statement,   turn on the kasa lightbulb when you have detected the hand

## Task 3.2 Enter/Leave Range Event


## Task 3.3 Trend Event

e.g. temperature decreasing over the past x minutes 


# Step 4 Control based on events