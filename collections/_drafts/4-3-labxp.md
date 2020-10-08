---
layout: course-page
title: 'Performance'
permalink: /draft/module4/labxp
description: 'Prototyping Connected Products - Lab Experiment 4'
labxp-of: id5415-4
introduction: In this Lab Experiment, we will reuse our lightbulb discovery function to detect smartphone on the network. It will provide an indicator 'at home' and 'away from home'. We will also explore the capabilities  wireless network infrastructure as a sensor to detect the presence of someone at home.we will test the capabilities of a network.
technique:
metrics:
report:
---

In this LabXp, we will add some sensory capabilities and implement contextual behaviour to control the light bulb based on our presence next to it (e.g knowing through our phone). Also we will touch upon the concept of MQTT and control someone else lightbulb through MQTT server.

---

- Do not remove this line (it will not be displayed)
  {:toc}

---

In our last assignment we have implemented the script `discover.py` to get the IP Address of our lightbulb and and connect to it automatically using Lightbulb class.

Now for this Lab Experiment, we have improvised the code of `discover.py` script so you can get all the devices connected to the network and it's ip.

Go to the link below and replace the code into your own `discover.py` script.

[Gist file for the discover script](https://gist.github.com/jackybourgeois/73766b1d3a5847ce03d135447ba77ba8)

# Step 1: Retrive your phone mac address

## Task1.1: improvise main.py

In your main.py, replace the calling of lightbulb function line:

```python
lightbulb_ip_address = find_lightbulb("192.168.1.1/24")
lightbulb = Lightbulb(lightbulb_ip_address, LIGHTBULB_THING_IP, LIGHTBULB_PRIVATE_KEY_PATH)
```

with this:

```python
    scanner = NetworkScanner("192.168.1.1/24")
    scanner.on_connect = on_device_connect_to_network
    scanner.on_disconnect = on_device_disconnect_from_network
    scanner.start_scanning()
```

Now in main.py, create and implement two more functions that will trigger on connect to the network

```python
def on_device_connect_to_network(device):
  print(device.mac)# print device name and it's Mac address connect to the network

def on_device_connect_to_network(device):
  pass
```

**Note** Make sure that your phone is connected to the same network as your light-bulb and current machine.

Now, if you run the main.py, it will print the MAC address of all the connected device to your connected network, including your phone and lightbulb.

## Task1.2

Follow[Step3]() from assignment4 to get vendors name, and then add it in `mac_vendor` dictionary defined in `discover.py` script.

# Step 2: Implement the contextual behaviour

Now that we have retrieved and stored our device details into `mac_vendor` dictionary, we can use this as a sensor contextualize different behaviour of light-bulb.

To do that you can improvise the the following tow methods that have implemented in `main.py`, and implement these two methods to infer the presence of the user

```python
def on_device_connect_to_network(device):
    print(device.name, device.mac)

    #condition to check the device name,
    if device.name == 'TP-Link':
        print('lightbulb back on the network')
        # create a lightbulb class object and print bulb status

    #condition to check the device name
    if device.name == 'iPhone':
        print('At home')
        # turn on the bulb

def on_device_disconnect_from_network(device):
    if device.name == 'TP-Link':
        print('Connection lost with the lightbulb')

    if device.name == 'Phone':
        print('Away from home')
        #Turn off the bulb
```

# Step 3: Flow-chart:

Now that we have improvise the code a lot better, let's visualize the code.

Create a flow chart of your code and introspect how did it improvise including the network scanning functionality.

Upload this flow-chart in your repository.

> **Merge and Push** Once you are done with your development and test cycle, do not forget to merge your branch into your master branch.

You can now try your code on the Raspberry Pi.

> **Updating the CHANGELOG file** In this assignment you have made significant additions to your prototype. Edit the file `CHANGELOG.md` and add what you have achieved in this assignment.