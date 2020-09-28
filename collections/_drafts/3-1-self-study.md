---
layout: course-page
title: "Sensors and time-series data"
permalink: /draft/module3/self-study
description: "Prototyping Connected Product - Self-Study 3"
self-study-id: 3
self-study-of: id5415-3
tags:
introduction: In this module, the self-study material focuses on sensors and the data they generate in the form of time-series. We will explore the most common sensors with their challenges and opportunities. We will introduce the concept of events as data point inputs are ingested by the system and trigger actions. Finally, we will distinguish between different options of data processing.
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---


# Sensors

<span class="mdi mdi-text-box-outline"></span> Reading (30 minutes)

A sensor transforms a physical condition into an electrical signal. This signal can be:

* analogue, a continuous measurement proportional to the physical condition
* digital, a discrete signal composed bits '0' and '1' representing the value of the physical condition

Have a look at this article from Codrey for an overview of the terminology and the main sensors:
[Codrey - Different types of Sensors](https://www.codrey.com/electronics/different-types-of-sensors/)

Covering sensors would be a course in itself and hardly relevant as it is highly driven by the purpose of the application.

As a designer, it is critical to have a list of criteria at hand to make your choice:

* Accuracy - how accurate should it measure in reflecting the physical condition?
* Environmental condition - What conditions should your it resists to (water-, sun-, baby-proof)?
* Range - What should be its minimum and maximum measured values?
* Calibration - Does it require calibration and how?
* Resolution - how precise should it be?
* Cost - How much should it cost?
* Repeatability - Does it worn out (after x measurements or overtime)?
* Physical - What a the form factors (sizes, weight, material)
* Security and Privacy - What are the implications of the output data on privacy?

Some of these questions might require to build a prototype and make some measurements.


# Time-series Data

<span class="mdi mdi-text-box-outline"></span> Reading (30 minutes)

The main characteristic of time-series data is its relation to time. Each data point (also called record or observation) is connected to a point in time, naturally ordering data by time. In the previous lab experiment, we wrote data into CSV files, starting with the first value of each row: the time.

This timestamp can take multiple forms depending on the data. Some dataset might only include the date without time. In this course, we use the so-called UNIX time. 

>Unix time is a system for describing a point in time. It is the number of seconds that have elapsed since the Unix epoch, minus leap seconds; the Unix epoch is 00:00:00 UTC on 1 January 1970. Leap seconds are ignored, with a leap second having the same Unix time as the second before it, and every day is treated as if it contains exactly 86400 seconds. Due to this treatment, Unix time is not a true representation of UTC. ([Wikipedia](https://en.wikipedia.org/wiki/Unix_time))

While being described as '__not a true representation__' of the Coordinated Universal Time (UTC), it is a widely used time format as it is free from timezone, daily saving and other time adjustments. It makes it convenient to exchange data.

Next to the time representation, a data point includes the value of each dimension. An example of time-series produced by a light sensor would include only one dimension representing the amount of light captured by the sensor. In contrast, a location sensor could collect the longitude and the latitude of an object at a given time, producing two dimensions.

Collecting and analysing time-series relies on the assumption that data points taken over time may have an internal structure to reveal. This structure can be analysed at different time scales. For instance, measuring the light in the room could be looked at as:

* direct measurement, momentary event: someone turned on the light in the room
* episode, pattern: inhabitants turning on the light every weekday around 6AM
* long-term trends: inhabitants turning on the light only in winter.

We will recognise in this course that data coming out of sensors can be visualised for overall insights. However, it is hard to go far without an algorithm processing this data to extract further insights. The main reason for this is the 4Vs mentioned in the first self-study material: the velocity, volume, veracity and variety of this data makes it hard to analyse by hand.

# Code Management

<span class="mdi mdi-text-box-outline"></span> Reading (30 minutes)

Managing code refers to its organisation from inside a single file to handling many files and folders over time. This organisation is vital for any project as its code grows organically (like a prototype). Inside a file, we structure code in `functions`, which typically groups a set of `statements` together to perform an action. Zooming out, each file of your project contains a set of `functions`. In Python, these files are called `modules` and their directories and sub-directories are called `packages`.

As you already experienced in the assignments and lab experiments, this organisation is practical to use the code of others by installing a package and import all of it or specific modules: the `import` statement at the top of the file.

## Class and Objects

Classes are part of programming style called Object-Oriented Programming (OOP) and a powerful way to further structure your code. It allows you to organise variables (information) and functions (behaviour) in relation to an object.

Let's take the example of our lightbulb. When we interact with the lightbulb, we deal with information such as its ON/OFF state and its level of brightness. We control the lightbulb behaviour using by 'turning it ON/OFF'. We might also 'ask' the lightbulb for its latest status. These form a set of variables and functions that are used together, revolving around the lightbulb. 

So far, we introduced data types such as numerical or textual values. In this context, classes are defining new data types.

* The `class` Lightbulb defines the information and behaviour of the data type 'Lightbulb'
* The ON/OFF states or brightness level, information that relates to the lightbulb, are `attributes` of this `class` 'Lightbulb'
* 'turn ON/OFF' or 'set brightness' are `methods` of this `class` 'Lightbulb'
* Each variable of type 'Lightbulb' is an `object` of this `class`. As it is a more complex data type than just a number or a text, we need a `constructor` that will build an `object` of type 'Lightbulb'

## Exceptions

When we write code, we want to avoid allow possible errors. You already noticed that Python will not start your program is there is an improper syntax.

However, some errors can not be avoided when writing the code. Writing data into a file which is not existing, reading data from an unresponsive sensor, losing the network connection. These are all examples in which the code can be correct but the program still crashes because things did not go as planned. We call `Exceptions` these errors taking place during the execution of a programme.

In the context of prototyping connected products, it is an important concept because our programme deals with hardware input and output as well as network interactions. To ensure that our programme continues running regardless of these errors, we need to 'handle these exceptions'. Regardless the programming language, exception handling includes 3 steps:

* We 'try' to execute a piece: typically our sensor reading or network interaction. With this 'try', we tell the programme that we are aware things could go wrong.
* We the behaviour of our programme if the code do not get executed as planned. For example, we could decide to ignore the issue and continue or we could try again.
* We do some housekeeping action at the end, whether it worked or not, such as closing a file or a connection.

# Quiz

Check your understanding with the following quiz! It is anonymous and you can try as many times as you want!

<iframe width="640px" height= "600px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=TVJuCSlpMECM04q0LeCIe-EN8Fz6eUZIqbayPT_HeNhUMTIzN1M2QzNNUVhFOElYMThFSkFYOUtTTy4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>