---
layout: course-page
title: "Network Technologies"
permalink: /draft/module4/self-study
description: "Prototyping Connected Product - Self-Study 4"
self-study-id: 4
self-study-of: id5415-4
tags:
introduction: In this module, the self-study material focus on the main network technologies allowing to transmit information from one device to another. While industry standards are quickly evolving, we will particularly look at selection criteria to make choices that fit what you are designing.
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---

# Network Terminology

`Network technologies` are all the elements that enable a machine to transmit information to another machine. In our lightbulb context, each student as a home network which we call a `local network`. All these networks are connected to `the Internet`, which is a network of (many) networks.

A `router` routes communication from one device to another. For instance, it receives a 'turn ON' message from the Raspberry Pi to be sent to the lightbulb, within the local network. It also routes information to the Internet, such as data to be saved on Bucket.

Each device on the network has a `MAC` address (for Media Access Control). This is a unique identifier of the device attributed by its manufacturer. A device with multiple network interface (e.g. WiFi and Bluetooth) will have a MAC address for each network interface.

The `IP address` (for Internet Protocol) is the identifier of a device on a network at a given time. In contrast with the MAC address, this address is attributed by the network and can change over time.

The hostname is the name of a device on a network. This name can only be used to reach the device on a small local network. On the Internet, because IP addresses are difficult to memorise, domain names are given to machines so that we can reach out for example google.com without memorising one of Google's IP addresses.

Bucket is a `server`, a computer like a personal computer connected to the Internet and providing a service such as storing data, serving the content of a website, processing information and so on. `Clients` are the consumers of these services. The complexity of each of those services (e.g. storing data) often requires a set of servers to work together, without clients being aware of this 'network of servers'. We refer to this `abstraction` as cloud infrastructure.

![IP Network Architecture](/assets/img/courses/id5415/module4/network_architecture.svg)

`Network protocols` are sets of rules and convention enabling machines to communicate through a network. It makes sure that both machine 'speak' the same language to understand each other's messages.

There are many aspects to agree about: what is the information? how is it physically transported? what is its destination? How is it protected? How is it compressed? All these aspects of network communication ruled by different protocols organised in layers. [OSI](https://en.wikipedia.org/wiki/OSI_model) is the reference model to organise these protocols in 7 layers from the physical layer (e.g. transmission over a LAN cable or WiFi) to the application layer (e.g. email, web browsing, video streaming).

As designers of connected products, let's focus on what we can influence.

# Physical network technologies

The physical layer is the most tangible part of the network: how is the information transmitted physically?

Beyond the choice between wired and wireless, there are physical network protocols for all type of applications. However, it is always a trade-off between these four criteria:

* Range: how far information can be sent (without a device receiving and transmitting again)? Postscapes gives [an overview of network protocol per range](https://www.postscapes.com/wp-content/uploads/2018/03/connectivity-diagram.jpg).
* Data rate: how much information can be sent per seconds?
* Power usage: how much power is required to send and receive information?
* Cost: how much data does it cost to set up the network infrastructure (if necessary) and to use it?

As designers, you are at the centre of these network technology choices. With your holistic view on the context and the problem to be addressed, you the key factors that should influence the decision.

* What is already in place, readily available? If not the perfect match, what would it cost to set up a different infrastructure? In this context, cost should be understood broadly as the effort by stakeholders to switch to this 'new' infrastructure. It ranges from user acceptability (e.g. switching from WiFi to a new protocol) to environment restrictions (e.g. hospital regulation on wireless technologies).
* What are the resource constraints? Is the product portable or connected to the power network? Can or should the information be processed on the device or sent to the cloud, with potential implication on the amount of traffic? How reactive should the device be, allowing or not the regular disconnection from the network?
* Does it need to be unidirectional or bidirectional communication? Some protocols are optimised for a specific application and have some uncommon limitation. For example, the LoRa protocol is designed for a long-range and a small amount of data. However, it has implications on the ability to get a response from the receiver.

# Network Topology

The network topology is how devices of a network connect or relate to one another. In this course, we emphasise the star topology as introduced in the first diagram in which devices communicate through a router (the centre of the start). All devices of the local network interact with a router which routes the traffic to its destination: another device on the local network or the next router on the Internet. It is a typical topology for a home, Internet-enabled environment.

However, there are other network topologies. For connected products, you might encounter `bus` and `mesh` topologies.

* `bus`: all devices connect to a common wire, the bus. A device sends a message on the bus and all other devices receive it. However, only the targeted device read the message. Examples are the bus CAN in cars or the C-Bus in building automation.
* `mesh`: devices are connected to several devices of the network (so-called nodes). They can receive and transmit messages from and to these devices. Only the targeted device read the message. Otherwise, the message is sent to the next devices. In IoT applications, it is often used to connect low-power devices with through wide area such as agriculture fields. For instance, sensors can send a message to its neighbour sensors, which will transmit the information further until reaching the destination. 

If all devices are connected to all devices, it is a fully connected network.

![Network Technologies](/assets/img/courses/id5415/module4/topologies.svg)

# Gateway and Hub

As we introduce different protocols, there is a need to connect networks of different protocols. This is the role of a `gateway` to translate information from a protocol to another. For example, you might use multiple sensors on your body to collect information about how you exercise, through the ANT or Bluetooth protocols. Your phone will play the role of a gateway to transmit this information through the Internet. Your home devices such as fridge, washing machine or light bulbs might communicate via Z-Wave or ZigBee. These devices require a gateway to connect to the Internet, e.g. to translate from these protocol to the Internet Protocol. A home `hub` play the role of a gateway among other tasks such as storing and processing data locally and controlling devices: the role of your Raspberry Pi in the course setting.

The following article presents three common types of devices architectures for connected products [Connected Product Architecture](https://medium.com/stanfy-engineering-practices/3-types-of-software-architecture-for-connected-devices-a-smart-light-bulb-case-54dc7727136f).


# The Message Queuing Telemetry Transport

In this course, we will explore the two major protocol to exchange messages on top of the Internet Protocol: HTTP and MQTT. We will dedicate the next module to HTTP, the fundamental protocol of the web technology.

Message Queuing Telemetry Transport (MQTT) is a protocol designed to send messages between devices. All messages go through an `MQTT server` which receives the messages to be published and transmit them to all subscribers. `MQTT clients` are all devices connecting to the MQTT server to publish or subscribe:

* `Publish` means sending a message a topic. For example, when the lightbulb `light1` is turned ON, its new status could be shared with the message 'on' on the topic '/light1/status'.
* `Subscribe` means registering the interest in receiving a message from a specific topic. In our example, the two other Raspberry Pi could subscribe to '/light1/status', thus both receiving the message 'on'.

![MQTT](/assets/img/courses/id5415/module4/mqtt.svg)

MQTT has two main advantages:

* the publishers (sending messages) and subscribers (receiving messages) are decoupled. It means that they do not need to know each other and never interact directly.
* the effort required to connect and exchange messages is small without compromising on security. This means that a limited amount of information is required in addition to the data to be sent. This is ideal for IoT application with often resource-constraint devices.
