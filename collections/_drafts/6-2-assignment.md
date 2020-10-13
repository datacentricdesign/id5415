---
layout: course-page
title: "Assignment 6"
permalink: /draft/module6/assignment
description: "Prototyping Connected Product - Assignment 6"
assignment-id: 6
assignment-of: id5415-6
introduction: In this assignment, you will
prog_environment: 
design: 
code_management: 
computational_concepts: 
---


Assignment 6 focuses on product analytics


---

* Do not remove this line (it will not be displayed)
{:toc}

---

# Product Analytics 

Having now reached this stage of our prototypes, we can now abstract away and analyse our products. 
What kind of patterns can we find from analysing all of our collected data? what is the best way to represent trends? 
What can we do with this information?
These are the sorts of question we should now be asking. 

## Step 1 - Take stock of available data 

First we must think and get an accurate account of all the aggregated data we possess. Create a table/list of all sensor inputs, data sources (such as emitted events & other properties), currently deployed controllable actuators in the prototypes, as well as the time period this data collection occured for each source.  

## Step 2 - What could you get from this data?
Now, with a rough indication of what is available to you, you can, vis-a-vis the good night lamp concept,
and indeed other connected products, we can start to think what kinds of trends we can retrieve and analyse. 
Make a list of possible trends/analytics (at least 3 more than the examples) that we can analyse using components of our table (step 1)
Possible examples :  
* What is sensor with most variance in its data? ( what do sensors that exhibit extremely low variance of range of values indicate?)
* What are the main periods of activity of our prototypes over a day, a week, and the entire period?
* what are the most dominant events triggered by the system? Is it constant over time?
* are the temperatures/light values consistent with day/night cycles - why/why not
* when are the peaks of activity of your various sensors? are they related to eachother?
* Are there any consistent  time patterns in device detections using the network (during the day, during the week)?

## Step 3 - Pre-Analysis
Given our work and experience with the prototype so far, and this list of possible information we could extract, go over each of the examples
(and your own), and briefly explain, before looking at the data, what you might predict is the answer to these questions, and how feasible
you estimate geting an answer to these questions with your current data is. 

## Step 4 - Relating possible trends and insights with prototype
At this point we have some enquiry points (step 2), some preliminary analysis of these, with perceived weaknesses, and the current status of the prototype given the initial vision concept. Before we try to answer these questions, we must first relate to their **relevancy** towards the development or analysis of this product. Going back to the initial concept of the course (in introduction) and its goals,  for each of these questions, relate them to this and the relevant components of the system. 
Example: 
A key point of the good night lamp is subtle non direct interaction between houses. knowing the peaks of activity between prototypes and how they relate to eachtother allows us to know if these products are in sync ( are we interacting mostly at the same time, or are most events missed by the other party?)  when several devices are active at the same time,  can we devise new interactions? 
On the other side, is the product working properly? Is the rate/distribution of events reasonable? 
