---
layout: course-page
title: "Performance"
permalink: /draft/module4/labxp
description: "Prototyping Connected Products - Lab Experiment 4"
labxp-of: id5415-4
introduction: In this Lab Experiment, we will reuse our lightbulb discovery function to detect smartphone on the network. It will provide an indicator 'at home' and 'away from home'. We will also explore the capabilities  wireless network infrastructure as a sensor to detect the presence of someone at home.we will test the capabilities of a network.
technique:
metrics:
report:
---

Some intro

---

* Do not remove this line (it will not be displayed)
{:toc}

---


# Phone discovery


same discovery process, this time for the phone
infer the presence of the user
update a property at home / not at home


# signal strength and bandwidth



controlling light of others via MQTT



on Bucket, create a group
	share property to this group
	subscribe to the MQTT group topic (to receive update from others)
	publish on this group (to share status)