---
layout: course-page
title: "Connected Products and Prototyping"
permalink: /module1/self-study
description: "Prototyping Connected Product - Self-Study 1"
self-study-id: 1
self-study-of: id5415-1
tags:
- prototyping
- IoT
- IoT stack
- connected product
- thing
introduction: What are connected products and how do their prototypes differ from other products? These are the questions we address in this self-study material. First, we define what is a Thing and a Network, to help us understand the opportunities and challenges of connected products. In this context, we discuss the process and techniques of prototyping connected products. Second, we introduce the concept of the Internet of Things, the technology backbone of connected products. Through a series of short videos, we illustrate the five layers that power the magic of connected products.
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---


# Prototyping Connected Products

<span class="mdi mdi-text-box-outline"></span> Reading (30 minutes)

In this course, we refer to connected products as any physical entity with the ability to share or receive information through the Internet. It is an interface between the physical and the digital world. Let's define the two pillars of connected products: `Things` and `Networks`

The term `Thing` is commonly used in reference to the technology enabling physical entities to have a digital manifestation. We commonly distinguish four types of Things (1):

![Four types of Things](/assets/img/courses/id5415/module1/four-types-of-things.svg)

* **Multipurpose computers** are computers with powerful resources such as Personal Computers, laptops or smartphones. They have capabilities to process a significant amount of information while handling multiple applications in parallel. These devices are complex digital entities on their own and often not considered as Things. They have their dedicated Industry with design and engineering.
* **Specialised embedded devices** are small computers, still with the ability to work on their own for a specific set of functionalities. They have processing capabilities to handle several tasks. They can connect to the Internet on their own or require a connection with a multipurpose computer. Typical examples are smart thermostats which connect to the home WiFi network or smart watches which interact with a smartphone in order to access the Internet. Design challenges lie in software components, such as handling complex interaction data and running 
* **Connected sensors and controls** focus on a specific functionality such as collecting the ambient temperature (sensing) or turning ON/OFF the light in the room (controlling). They deal with limited resources (e.g. energy, processing, memory). Design challenges lies at the frontier of software and hardware, often achieve with Arduino-like platform.
* **Passively trackable object** are pieces of metal (e.g. RFID tags) or visuals (e.g. QR code for 'Quick Response' code) which are attached to hardware, enabling the link with their digital representation. They require an external device to establish this connection. A typical example is a delivery parcel with QR Code. Throughout the delivery process, the parcel is scanned so that the customer can trace its location online (digital representation).

In this course, we will use a [Raspberry Pi](/tags/#raspberry-pi) to represent a specialised embedded device: a home hub. It is a small multipurpose computer which makes it a good candidate for rapid prototyping of connected products. In contrast, the light bulb of our prototyping kit is best representing the type of Things 'connected sensors and controls'.

The common point of all these `Things` is their purpose of digitising and communicating information. Thus, the `Network`, establishing the communication between these `Things`, is the second pillar of connected products. How this communication is established, what constraints does it imply? How fast, reliable, secure is this communication? As communication is at the core of a connected product, it becomes critical to take it into consideration throughout the design process. We will dedicate the [fourth module](/module4) to Network technologies

The field of connected products is growing fast as it enables three major opportunities:

* **Data** -- the Internet of Things is all about generating data. In this context, we often refer to the 5Vs as depicted in by the [IBM infographic](https://www.ibmbigdatahub.com/infographic/extracting-business-value-4-vs-big-data): Volume, Velocity, Veracity, Variety and more recently Value.
* **Intelligence** -- establishing a connection between devices makes them more aware, more capable, and by extension more intelligent. Thus, the common phrasing of 'Smart'. However, it all comes down to the ability to connect and share information, so that each part of the system can gain and leverage knowledge.
* **Continuous Interactions** -- A connected `Thing` cannot only interact with the nearby environment. It keeps the link to the supply chain open. It changes the relationship between designers and their products. Designers can apply [continuous engineering](https://www.ibmbigdatahub.com/blog/what-continuous-engineering) methods to learn about the performance and usage of their product to continuously redesign and improve them.

However, these opportunities naturally lead to three key challenges:

* **Complexity** -- in contrast with a traditional product made of a single device, a connected product potentially involve several devices including some devices which are not under your control. It forms a system in which pieces of codes are running on many different devices, which makes the maintenance more difficult, but also raise new questions regarding privacy, security and ownership;
* **Dynamic** -- this network of Things is highly dynamic. The Things available on the network at a given time might not be available later on, or in a different form: the location changed, the electrical power is gone, the machine learning algorithm gathered more data and thus behave differently.
* **Inter-dependence** -- without its connection to a network, a Thing is often no longer able to deliver its functionalities. The data it receives is often the main driver of its behaviour. Thus the data quality shape the final What should the thermostat do when no temperature data is coming?

With these opportunities and challenges in mind, what is different about prototypes of connected products?

It is difficult to leverage these opportunities or measure the impact of these challenges without experiencing them. While implementing a functional prototype is a common practice in the later stages of design processes, it is also becoming necessary in the earlier stages.

> The overarching purpose of any form of prototype is to learn something at a much lower cost in terms of time and effort than building out a product. Marty Cagan. Inspired (2)

Here are four examples:

* Feasibility prototypes -- Can we really make it? How accurate and reactive would it be? Alone or together with engineers, it is critical to test the feasibility of your concepts as it will impact the outcome. Algorithm, performance, scalability, fault tolerance, new technology, third-party components, all are potential technical risk your team must address.
* User Prototypes -- You are probably familiar with wireframe prototype. Augmenting this wireframe with code enables you to test your user interaction further, by simulating how your product would react and how this reaction impacts your users.
* Live-Data Prototypes -- When dealing with data in a new context, we often have misconceptions regarding how the data will look like, or no idea at all. As data is often the main driver of your connected product, being able to experience live data is critical to give it the appropriate role in your concept. The live-data prototype also helps you collect evidence.
* Hybrid Prototypes -- You might find yourself combining several of the three types of prototypes above. An example would be a Wizard-of-Oz prototype which enables you to better understand the user interaction as well as collecting live data. 

# Internet of Things' Technology Stack

<span class="mdi mdi-video"></span> Video series (30 minutes)

We unpacked the particular prototyping needs for the design and development of connected products. These needs come from the Internet of Things, or the IoT, which is the technology behind product-service systems. Like an iceberg, the physical ‘Things’ are the tip, visible above water while most of the technology and complexity is hidden, immersed underwater.

In the following series of seven small videos, we shed light on this digital technology, mapping its literacy through a layered framework so-called 'stack'. We explore what is the Internet of Things, extracting the key roles of designers in the development of product-service systems.

[Video Series on the Internet of Things' Technology Stack](https://www.youtube.com/playlist?list=PL3sV9hKiYEP-MVdxCXYfl7vei77xdbJo6)

# Check your Understanding

<span class="mdi mdi-head-question"></span> Quiz (15 minutes)

Check your understanding with the following quiz! It is anonymous and you can try as many times as you want!

<iframe width="640px" height= "600px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=TVJuCSlpMECM04q0LeCIe-EN8Fz6eUZIqbayPT_HeNhUNUFFMUxIMkxGN1Q5NFhSTDBSUTY4V0pNVS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>

**References**

1. [Designing Connected Products](https://www.oreilly.com/library/view/designing-connected-products/9781449372682/) UX for the Consumer Internet of Things. By Clair Rowland, Elizabeth Goodman, Martin Charlier, Ann Light and Alfred Lui.
2. [Inspired](https://www.goodreads.com/book/show/35249663-inspired) How to create tech products customers love
3. [Daniel Elizalde’s IoT Technology Stack](https://danielelizalde.com)
 [Google Nest Learning Thermostat](https://store.google.com/us/product/nest_learning_thermostat_3rd_gen)
4. [Pentoz Connectivity and Network Technologies](https://pentoztechnology.wordpress.com/2018/04/04/connectivity-and-network-technologies-of-iot/)
5. [Google Cloud Dataflow in the Smart Home Data Pipeline](https://nest.tech/google-cloud-dataflow-in-the-smart-home-data-pipeline-5ae71781b856) by Matt & Riju (Medium)
6. [Atech ISO Model](http://aurumme.com/atech/osi-model/3/)
7. [3 Types of Software Architecture for Connected Devices.](https://medium.com/stanfy-engineering-practices/3-types-of-software-architecture-for-connected-devices-a-smart-light-bulb-case-54dc7727136f) A Smart Light Bulb Case by Pablo Bashmakov (Medium) 
8. [IoT data Silos](https://www.slideshare.net/rajrsingh/iot-meets-geo)
9. [EVRYTHNG Cloud integration](https://www.slideshare.net/rajrsingh/iot-meets-geo)
10. [Design Thing, Nilsen Norman Group](https://www.nngroup.com/articles/design-thinking/)