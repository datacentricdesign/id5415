---
layout: course-page
title: "Analytics Dashboard"
permalink: /draft/module6/assignment
description: "Prototyping Connected Product - Assignment 6"
assignment-id: 6
assignment-of: id5415-6
introduction: In this assignment, we will design an analytics dashboard for our prototype of Good Night Lamp. We will make an inventory of data collected throughout the course for our prototype as well as for the whole classroom. We will establish what information is missing, which would need to be gathered in future iterations.
prog_environment: 
design: 
code_management: 
computational_concepts: 
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---


Having now reached this stage of our prototypes, we can now abstract away and analyse our products. 
What kind of patterns can we find from analysing all of our collected data? what is the best way to represent trends? 
What can we do with this information?
These are the sorts of question we should now be asking. 

# Step 1 - Take stock of available data 

First we must think and get an accurate account of all the aggregated data we possess. Create a table/list of all sensor inputs, data sources (such as emitted events & other properties), currently deployed controllable actuators in the prototypes, as well as the time period this data collection occured for each source.  

# Step 2 - What could you get from this data?
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

# Step 3 - Pre-Analysis
Given our work and experience with the prototype so far, and this list of possible information we could extract, go over each of the examples
(and your own), and briefly explain, before looking at the data, what you might predict is the answer to these questions, and how feasible
you estimate geting an answer to these questions with your current data is. 

# Step 4 - Relating possible trends and insights with prototype
At this point we have some enquiry points (step 2), some preliminary analysis of these, with perceived weaknesses, and the current status of the prototype given the initial vision concept. Before we try to answer these questions, we must first relate to their **relevancy** towards the development or analysis of this product. Going back to the initial concept of the course (in introduction) and its goals,  for each of these questions, relate them to this and the relevant components of the system. 
Example: 
A key point of the good night lamp is subtle non direct interaction between houses. knowing the peaks of activity between prototypes and how they relate to eachtother allows us to know if these products are in sync ( are we interacting mostly at the same time, or are most events missed by the other party?)  when several devices are active at the same time,  can we devise new interactions? 
On the other side, is the product working properly? Is the rate/distribution of events reasonable? 



# Step 1 Share data with the class

By default, Bucket makes properties' data only available to the owner of Things and the Things themselves. In Module 5, you created a group to enable your teammates' Things to subscribe (i.e. to access) your lightbulb's status via MQTT.

We will use the same process to share properties with the whole classroom. For this, the course coordinator created a group `id5415-20-q1`.

>**In this step, you can make the data of your Things available to the students and teachers taking part in this edition of the course. This data will be linked to the name and id of your Thing, as well as your Bucket username. It is your personal decision to share this data which as no impact on your assessment for the course nor your ability to proceed in this assignment. Whether you go forwards with sharing none, one or several properties' data, please document your decision process in the lab experiment.**

On Bucket, go to your Thing properties (e.g. the lightbulb status of your Lightbulb Thing). In the 'Sharing' card, fill in the name of the group `id5415-20-q1` and select 'Group' in the drop-down menu. Finally, by clicking on the blue button 'Grant', you will be granting access to this property to the member of the group.

![Grant a group](/assets/img/courses/id5415/module6/assignment/1_1_1.png)

The result should look as follows, your property now listing a granted access to the group.

**Feel free to revoke the access at any time by clicking on the orange button Revoke**

![Granted](/assets/img/courses/id5415/module6/assignment/1_1_2.png)

You can repeat this step for all the properties we want to share, such as the three sensors (light, temperature and humidity), the Processor Usage of the Raspberry Pi and the network status.

**Let's keep the IP address private as it can reveal more sensitive information.**

In the left panel of Bucket, navigate to 'Shared properties'. In this page, you should see an expanding list of shared properties belonging to your peer students and course teachers. We will look at these chart in Step 3, leaving all students who want the time to share their properties.

![Shared properties](/assets/img/courses/id5415/module6/assignment/1_1_3.png)

>If you do not see any shared properties, you are probably not yet a member of the group for the current edition of the course (id5415-20-q1). Please contact the course coordinator via MS Teams with your Bucket username, so that you can be added to the classroom group.

# Step 2 Design an Analytics Dashboard

Let's step back from our code and prototype and think about the Good Night Lamp concept.

Metrics

what data is relevant for the Good Night Lamp, to capture product and user behaviour.

a specification of data collected and metric to look at

a draw of expected data

a description of potential insight to extract

# Step 3 Make an 





	Ask students to share their property in the Bucket group id5415-q1-20, I would add all students in this group
	Inventory: looking at the graph mentioned above (count of data points per type in properties shared across the cohort), along the line you described: what is available... What could we get out of this data, what is missing.