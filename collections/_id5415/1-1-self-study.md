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

<span class="mdi mdi-text-box-outline"></span> Reading (15 minutes)

In this course, we refer to connected products as any physical entity with the ability to share or receive information through the Internet. It is an interface between the physical and the digital world. Let's define the 2 pillars of connected products: `Things` and `Networks`

The term `Thing` is commonly used in reference to the technology enabling physical entities to have a digital manifestation. We can distinguish 4 types of Things:

* **Multipurpose computers** are computers with powerful resources such as Personal Computers, laptops or smartphones. They have capabilities to process a significant amount of information while handling multiple applications in parallel. These devices are complex digital entities on there own and often not considered as Things. They have their dedicated Industry with design and engineering.
* **Specialised embedded devices** are small computers, still with the ability to work on their own for a specific set of functionalities. They have processing capabilities to handle several tasks. They can connect to the Internet on their own or require a connection with a multipurpose computer. Typical examples are smart thermostats which connect to the home WiFi network or smart watches which interact with a smartphone in order to access the Internet. Challenges 
* **Connected sensors and controls** focus on a specific sensing or control functionality such as collecting the ambient temperature or turning ON/OFF the light in the room. They deal with limited resources (e.g. energy, processing, memory). Design challenges lies at the frontier of software and hardware, often achieve with Arduino-like platform.
* **Passively trackable object** are pieces of metal (e.g. RFID tags) or visuals (e.g. QR code for 'Quick Response' code) which are attached to hardware, connecting them with their digital representation. They require an external device to establish this connection. A typical example is delivery parcels with QR Codes. Throughout the delivery process, parcels are scanned so that customer can trace their location online (digital representation).

 In this course, the [Raspberry Pi](/tags/#raspberry-pi) represents such a Specialised embedded device. It is a small multipurpose computer which makes it a good candidate for rapid prototyping of connected products. The light bulb is best representing this type of Things. 

The common point of all these Things is their purpose of digitising and communicating information. Thus, the `network`, establishing the communication between these Things, is the second pillar of connected products. How this communication is established, what constraints does it imply? How fast, reliable, secure is this communication? As communication is at the core of a connected product, it becomes critical to take it into consideration throughout the design process. We will dedicate the [fourth module](/module4) to Network technologies

The field of connected products is growing fast as it enables 3 major opportunities.

* Interactions:

continuous engineering

* Data: the Internet of Thing is all about generating data 

* Intelligence: establishing a connection between devices makes them more aware, more capable, and by extension more intelligent. Thus, the common phrasing of 'Smart'. However, it all comes down to the ability to connect and share information, so that each part of the system can gain and leverage knowledge.

However, these opportunities naturally leads to three key challenges:

* Complexity: in contrast with a traditional product made of a single device, a connected product potentially involve several devices including some devices which are not under your control. It forms a system in which pieces of codes are running on many different devices, which makes the maintenance more difficult, but also raise new questions regarding privacy, security, ownership;
* Dynamic: this network of Things is highly dynamic. The Things available on the network at a given time might not be available later on, or in a different form. Connected to a network, Things gain the ability to upgrade themselves even without artificial intelligence.
* Inter-dependence: depend on the network to reach out, on the data it receives and its quality

With this in mind
What is different about prototypes of connected products?


* Feasibility prototypes
* User Prototypes
* Live-Data Prototypes
* Hybrid Prototypes

* Testing techniques

* What do we mean by prototype
* What for?

## References

1. [Designing Connected Products](https://www.oreilly.com/library/view/designing-connected-products/9781449372682/) UX for the Consumer Internet of Things. By Clair Rowland, Elizabeth Goodman, Martin Charlier, Ann Light and Alfred Lui.

# Internet of Things' Technology Stack

<span class="mdi mdi-video"></span> Video series (30 minutes)

We unpacked the particular prototyping needs for the design and development of connected products. These needs come from the Internet of Things, or the IoT, which is the technology behind product-service systems. Like an iceberg, the physical ‘Things’ are the tip, visible above water while most of the technology and complexity is hidden, immersed underwater.

In the following series of seven small videos, we shed light on this digital technology, mapping its literacy through a layered framework so-called 'stack'. We explore what is the Internet of Things, extracting the key roles of designers in the development of product-service systems.

[Video Series on the Internet of Things' Technology Stack](https://www.youtube.com/playlist?list=PL3sV9hKiYEP-MVdxCXYfl7vei77xdbJo6)


# Check your Understanding

<span class="mdi mdi-head-question"></span> Quiz (15 minutes)

Check your understanding with the following quiz! It is anonymous and you can try as many times as you want!

<iframe width="640px" height= "600px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=TVJuCSlpMECM04q0LeCIe-EN8Fz6eUZIqbayPT_HeNhUNUFFMUxIMkxGN1Q5NFhSTDBSUTY4V0pNVS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>