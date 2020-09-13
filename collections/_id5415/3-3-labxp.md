---
layout: course-page
title: "Sensor Data Collection"
permalink: /module3/labxp
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







# Step 1: Do some light processing of raw sensor data

## Task 1.1: Create an "environment" dashboard

1. Create a new temperature property, for storing temperatures in Fahrenheit 
2. From raw celsius temperature data, convert to Fahrenheit and send this value to this new property
3. Create a temperature dashboard with temperature in Celsius, Fahrenheit, and humidity. 

# Step 2:  Explore the LightSensor Class to trigger custom events

1. Besides the functions we've defined, you can adjust the threshold (and add trigger functions) directly in the [LightSensor  class](https://gpiozero.readthedocs.io/en/stable/api_input.html#lightsensor-ldr):
   1. In your physical setup, adjust the threshold value (passed when creating the class), so that it detects internally when you pass your hand over the LDR. 
   2. Create two new functions "hand_detected" and "no_hand_detected" , and set your LightSensor object "when_dark" and "when_light" properties to the proper function, eg: `LDR_sensor.when_dark = MyFunctionName" `
   3. In these new functions, print a corresponding statement to your console, e.g. "Hand detected" 
   4. Besides the print statement,   turn on the kasa lightbulb when you have detected the hand 



