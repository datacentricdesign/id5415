---
layout: course-page
title: 'User Interaction'
permalink: /draft/module5/labxp
description: 'Prototyping Connected Products - Lab Experiment 5'
labxp-of: id5415-5
introduction: In this Lab Experiment,
technique:
metrics:
report:
---

In this Lab experiment, Step1 would be designing & explaining a proper webservice and specify which HTTP protocol would be appropriate for each of the API call. In step 2, we will see how to connect to someone else THING in your group and retrive the information uisng MQTT server.

---

- Do not remove this line (it will not be displayed)
  {:toc}

---

TODO full Lab XP

# Step 1 Design a Web-Service

In [Assignment 5](https://id5415.datacentricdesign.org/module5/assignment), we have created a web-service with local webserver and implemented an API call to the `blink()` method of the lightbulb class. Using this we have learnt to control our lightbulb (calling particular method from lgihtbulb class) from any devices that connected to the same network.

Now that we have web server working, the next task for you in this LabXP is:

- Explain the system architechture of the current lightbulb system, with all the classes together.
- Specify different API calls for web-server to use each of the fucntionality provided by these classes. e.g. API call to blink method in lightbulb class.
- Explain why this API is necessary to implement
- Read abou the different HTTP protocol and expalin how will you use this in your API calls. E.g to get the current value of sensor we should use `GET` request method.
- Think about the services that can have privacy issue, explain which are are those and how would you prevent/exclude that from your API calls.

In your API calls, you can add as many functionality as you want besides the current functions you have implemented.

**NOTE**You do not have to implement the API calls neither code it, but only need to explain which API would be useful and how it will connect to over all system together.

# Step 2 Connect Homes via MQTT

In this step, we use the MQTT protocol to listen to the lightbulb status of the other team member. This enables each lightbulb to react based on the behaviour of other lightbulbs, realising the mechanism of the Good Night lamp.

## Task 2.1 Create a group (once per team)

Bucket includes an MQTT server allowing Things to publish the updated values of their Properties. This is how our Python code has been sending data to Bucket throughout the course. Similar to the HTTP REST API, the server receive values of a Thing on the topic:

/things/[THING ID]/properties/[PROPERTY ID]

However, for both security and privacy reasons, we can only publish and subscribe on this topic if we are the Thing (with the private key) or if we are the owner of the Thing (with our user credentials). If we want others to see the data of our Properties, we need to explicitly grant them access. We will do this in two stages:

1. we create a group and add the things of all team members in this group
2. we grant access of each LIGHTBULB_STATUS property to all members of the group

On Bucket, in the top-right corner, click on the user icon > Profile Settings. You will be prompted to sign in for the management page of your DCD account. In the left panel, select 'Groups'.

On the page, you should see a list of groups that you are in, namely 'public' and 'user', giving you access to all things that are shared publicly or to regular users (as opposed to 'administrator').

Below this list, enter the name of a new group (avoid spaces or special characters). Then, click the green button 'Create a new group'.

The group should appear in the list of groups you are in, as well as a setting card with the option to add members to this group. The creator of the group is automatically added to this group and the only one who can see the setting card for this group.

On this setting card, select 'Thing' in the dropdown menu, and paste the thing ID of your lightbulb. Click on the blue button 'Add to group'. Repeat this process with the Thing ID of your teammates' lightbulb.

At this stage, we still do not share any properties. We have a group of Things, making it easier to share properties with all members of that group.

## Task 2.2 Share properties with the group

Now, each team member can go back to Bucket and the Lightbulb Thing they associated to the newly created group. Click on the property of type LIGHTBULB_STATUS.

In the sharing settings of this property, enter the name of your group, select 'group' in the dropdown menu and click 'Grant'. This should add the group to the list of granted subjects with 'read' access.

All things in the group should be able to access the data of this property now.

In the settings of your thing, check if you can see all three properties listed in the card 'Property Accesses'.

## Task 2.3 Search for shared properties

Back to VS Code and Python, let's load the virtual environment.

As we updated our SDK, you will also need to uninstall and reinstall it.

```bash
pip uninstall dcd-sdk
pip install dcd-sdk
```

For this lab experiment, we can create a file `shared_property_handler.py` with the following code:

```python
from dcd.bucket.thing import Thing

my_thing = None

def main():
    # Instantiate a thing with its credential
    # By default, looking into .env for THING_ID and PRIVATE_KEY_PATH (default "./private.pem")
    my_thing = Thing()
    shared_properties = my_thing.find_shared_properties()
    # Show a message if we did no find any shared properties
    if len(shared_properties) == 0:
        print("No shared property found.")

    for prop in shared_properties:
        # Show each property found
        prop.describe()

if __name__ == "__main__":
    main()
```

In this code, we create our Thing object and use its methods find_shared_properties().

Execute this code to check if the property sharing worked as intended. You should see a list of three properties.

## Task 2.4 Subscribe to updates

Now that we have (read) access to the properties of our teammates, we can subscribe to their value updates. Let's first move this initial code in a separate function `subscribe_to_other_lightbulbs()`.

```python
def main():
    my_thing = Thing()
    subscribe_to_other_lightbulbs()
```

Our function should look like this:

```python
def subscribe_to_other_lightbulbs():
    """Search for all properties shared with our Thing and subscribe to MQTT messages from those of type LIGHTBULB_STATUS
    """
    # search for shared properties, leave empty for all
    # add "dcd:group:..." to target shared properties within a specific group.
    shared_properties = my_thing.find_shared_properties()
    # Show a message if we did no find any shared properties
    if len(shared_properties) == 0:
        print("No shared property found.")
    # Select only the properties of type LIGHTBULB_STATUS and not from our Thing
    for prop in shared_properties:
        # Show each property found
        prop.describe()
```

From here, there are three tasks to be done:

1. Filter out our own properties and properties that are not of type LIGTHBULB_STATUS. We do not want to listen to our own updates, even though our property is shared in this group.
2. Build the MQTT topic as described in the first Task, composed of /things/[thing id]/properties/[property id]
3. Set a handler, with the same principle as our sensor or network handler, we want to define what to do when we receive an update from the team members' lightbulb.
4. Subscribe to the MQTT topic so that we start receiving updates.

These translate in the following lines of code to add to our for-loop (to be done for each shared property)

```python
        if prop.type_id == "LIGHTBULB_STATUS" and my_thing.thing_id != prop.thing.id:
            # Prepare the MQTT topic to subscribe
            topic = "/things/" + prop.thing.id + "/properties/" + prop.property_id
            # Set the handler on this topic
            my_thing.mqtt.mqtt_client.message_callback_add(topic, other_lightbulb_handler)
            # Subscribe to the topic
            my_thing.mqtt.mqtt_client.subscribe([(topic, 1)])
```

## Task 2.5 Handle value updates

Before executing the code, we need to define the function we specified as the handler, namely `other_lightbulb_handler`.

We need to add `import json` at the top of the file so that we can manipulate JSON object easily. Here is an example of handler function It prints the MQTT topic on which the message was received, which tells us about the thing and the property it comes from. It also loads the message as a JSON structure and prints it.

```python
def other_lightbulb_handler(client, userdata, msg):
    """Handle events from other lightbulbs
    Args:
        client: the MQTT client instance for this callback
        userdata: the private user data
        msg (MQTTMessage): an instance of MQTTMessage.
    """
    print("Event from an other lightbulb!")
    print("Topic: " + msg.topic)
    jsonMsg = json.loads(msg.payload)
    print("JSON message: " +str(jsonMsg))
```

Executing the code, not much should happen until someone in the team updates its lightbulb status.

You can replace the print() statement with a call to your lightbulb and start controlling your lightbulb based on the status of others'.

> **Report** On GitHub, in your lab experiment report, draw a diagram to capture the interaction between the three lightbulb Thing through MQTT. Describe what you tried and what opportunities you can envision with this mechanism.
