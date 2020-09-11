---
layout: course-page
title: 'Setting Up Tools'
permalink: /module1/assignment
description: 'Prototyping Connected Product - Assignment 1'
assignment-id: 1
assignment-of: id5415-1
introduction: Assignments are where the prototyping happens. In this first assignment, you will set up a Raspberry Pi as a connected home hub to control connected light bulbs. We will walk you through the steps while exploring the purpose of each component, incrementally drawing a product architecture.
prog_environment: Raspberry Pi
design: Architecture
code_management:
computational_concepts: Data
tags:
- microprocessor
- microcontroller
- IDE
- CLI
- SSH
- Bucket
- setup
- data
- logs
- architecture
- Raspberry Pi
---

We have prepared 5 steps for this assignment (each again divided into specific tasks you have to perform).

---

* Do not remove this line (it will not be displayed)
{:toc}

---

# Step 1: Set up your cloud and prepare a configuration for your Raspberry Pi

A core component of the technology stack of IoT is the `cloud': the location somewhere on the internet where we keep the data we want to use and the tools that determine how we will use this data. It will not be until module 5 before we start using could data and tools in the assignments. However, in this first assignment, we will prepare for that by setting up the cloud.

## Task 1.1 Activate your space in 'Bucket".

Our Data-Centric Design Lab (DCD Lab) has prepared a space online where it is easy for the students in this course to create their cloud. It is called [Bucket](https://dwd.tudelft.nl/bucket). The following video gives you a little tour to familiarize yourself with Bucket. To access and use it, you will create your DCD Lab account.

<iframe width="560" height="400" src="https://www.youtube.com/embed/H2Ogmi1J-P8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Task 1.2 Setup your Raspberry Pi as a Thing in your cloud and create a Disk Image

Once you log into Bucket, the dashboard shows an empty page with a form to create a `Thing'. In the terminology of IoT a 'Thing' refers to any physical or virtual entity that is connected to the internet and is involved in the exchange of data. To be able to do something with data using our Raspberry Pi, we need it to show up here as a thing and therefore we will create its digital twin here. Type in a name and a description, and select the type 'Raspberry Pi'.

You will notice many settings will appear. We will now go through the correct settings and gather these settings into a disk image, that you will later blend into your Raspberry Pi through a USB stick for it to be configured properly (so that you can start working with it). Keep in mind that **we do not store any of this information. Only you, the owner of the Thing can create and download the settings through a secured connection**.

The first section of settings is about restricting access to your Raspberry Pi. As you will store network information on your Raspberry Pi, it is essential to set it up in a way that prevent others to access it.

![Create a Thing with WiFi Credentials connecting to the internet](/assets/img/courses/id5415/module1/assignment/1_2_21.png)

The second section is about connecting to the network. Eduroam is an enterprise-grade network which requires several challenging interventions on the Raspberry Pi system. Filling in your NetId and password, we make sure that your Raspberry Pi can automatically connect to Eduroam and manage your credential properly. We conveniently provide a similar function for your home network.

![Home network](/assets/img/courses/id5415/module1/assignment/1_2_22.png)

![Eduroam](/assets/img/courses/id5415/module1/assignment/1_2_23.png)

Once you have filled in all the information in section 1 and 2, click the 'Create' button. The page should update with your newly created Thing. However, it will take a 'long' time to generate your disk image. You can see a status indicator which will turn into a 'Download' button when your disk image is ready.

You can proceed to the next step while waiting for your Raspberry Pi image to be ready.

# Step 2: Set up the 4 key tools for Prototyping Connected Product

To be able to do any programming through coding in the next module(s), we need to set up several tools that are able to interact with the connected light bulb of the prototyping kit. Let's take this opportunity to introduce and install the tools we will use throughout the course.

## Task 2.1: Installing a Version Control System (VCS)

{is this what you mean to point out: To be able to work as a team on coding, we need to have a collaborative environment suitable to manage that, called a VCS version Control System . We will use a VCS called Git}
The first tool is Git, a Version Control System (VCS). A VCS facilitate the management of and the collaboration around a piece of code. We will cover Git and its purpose for prototyping connected products in the next module. For now, its installation (especially on Windows) helps us automatically set up a development environment.

Download and install Git from the [official website](https://git-scm.com/download). On Mac you might go for the Binary installer option.

## Task 2.2: Installing an Integrated Development Environment (IDE)

Editing and executing code is done in an Integrated Development Environment (IDE), which simply is a text editor customised to code editing and execution. For this course, we will choose Visual Studio Code (or VS Code) which brings all the necessary functionalities without being overly complicated.

Download and install Visual Studio Code from the [official website](https://code.visualstudio.com/).

Create a folder on your computer (i.e Desktop) to store the files for this course. Open VS Code and click the top menu File > Open ... to open your course folder. Let's take a little tour of the IDE.

- The left panel relates to the management of your files with from the top: 1. the tree of your project files 2. searching into your project files 3. managing your file with Git. This is the whole menu of Module 2;
- The bottom panel relates to the execution of programs. We will especially look at the Terminal tab in the next task.

![Tour to VS Code](/assets/img/courses/id5415/module1/assignment/2_2.png)

<iframe width="560" height="400" src="https://youtube.com/embed/Sdg0ef2PpBw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Task 2.3: Get familiar with the Command-line Interpreter (CLI)

A CLI interprets the text you have entered into a command (e.g. create a directory, open an application, ...) - that's it! "What is the point?" you may wonder, as today we have many Graphical User Interfaces (GUI's) available that allow you to achieve many things as well, but without the cumbersome process of typing. Well, when prototyping you are developing technologies and playing with cutting edge ones, many of which simply do not have GUIs. Skill in a Command-Line Interpreter (CLI) is therefore not something you acquire just for one project but rather for your entire digital prototyping life!

In software engineering a number of varieties exists: 'Unix shell', 'Console', 'Terminal' or 'Command Prompt'. All of them refer to this black window full lines of text. Find it in VS Code: go to View > Terminal. It opens a tab in the bottom panel. In this course, we will use Unix shell as a command-line language, which comes in different flavours, the most common being `bash` and `zsh`. On the top right corner of the Terminal, a drop-down menu give you several options.

On Windows, the installation of Git should have install bash, **you must select bash instead of Power Shell.**

![Introduction to VSCode Terminal](/assets/img/courses/id5415/module1/assignment/2_3_0.png)

Each command line start with your username @ your machine name. Then, the current folder is shown

To execute a command simply type it, and press the `ENTER` key. Let's MaKe a Directory (Folder) named `test` with the command `mkdir`:

```bash
mkdir test
```

The new folder should appear in your file tree in the left panel. `ls` is the command to list files and directories. Type ls followed by `ENTER`:

```bash
ls
```

Your test folder should appear as a result of this command as shown below.

![](/assets/img/courses/id5415/module1/assignment/2_3_1.png)

Most commands come with a manual, available with the command `man`. Let's explore the option of the `ls` command.

```bash
man ls
```

![](/assets/img/courses/id5415/module1/assignment/2_3_2.png)

You can see a long list of option, starting with a dash `-`. These options can be combined together. For instance you will note `-G` for colouring the result and `-l` for the long and detailed result. To quit the manual, press `q`. Now we can try:

```bash
ls -Gl
```

![](/assets/img/courses/id5415/module1/assignment/2_3_3.png)

In the terminal, everything relates to where you are in the file tree, i.e. in which folder you are in. With the command `cd` you can 'change directory' to navigate this tree. The 'path' is the chain of directories to reach your targeted file or directory.

```bash
cd test
```

![](/assets/img/courses/id5415/module1/assignment/2_3_4.png)

You entered 'test' folder, you can notice the command line is now showing 'test' as the current folder. There are three important path markers: dot `.` for the current directory, dot dot `..` for the parent directory, and tilde `~` for the home directory.

Going back to the parent directory:

```bash
cd ..
```

![](/assets/img/courses/id5415/module1/assignment/2_3_5.png)

Finally, the tilde `~` leads you to your home directory:

```bash
cd ~
```

![](/assets/img/courses/id5415/module1/assignment/2_3_6.png)

To avoid typing the same command again and again,you can press the Arrow-Up key to bring back you previous commands.

Read this [Bash cheat sheet](https://www.educative.io/blog/bash-shell-command-cheat-sheet) for some additional basic commands.

## Task 2.4: Installing a Python Programming Language

The last tool we need is Python, the programming language for this course.

On Windows, download and install from [here](https://www.python.org/downloads/windows/). Navigate to 'Latest Python 3 Release' and scroll down to file a Windows installer.

On Mac OS, download and install from [here](https://www.python.org/downloads/mac-osx/). Navigate to 'Latest Python 3 Release' and scroll down to file a Windows installer.

Restart VS Code. open your course folder and open a Terminal. Check your Python installation with:

```bash
python3 --version
```

This command should return a version number of Python 3.x.x. From now, whenever we want to use python from terminal, we can start the command with `python3`

## Task 2.5: Setting up Virtual Environment to work

A virtual environment is a self-contained directory tree that contains a Python installation for a particular version of Python. It also includes additional packages (modules/libraries) to meet the requirements of each python application we are developing. Different python application can use different virtual environments.

To create a virtual environment, Go to VSCode and open the terminal. Type below two command to first install the `virtualenv` and then create a virtual environment called `venv`.

```bash
pip3 install virtualenv

python3 -m virtualenv venv
```

VS Code recognises the creation of this new environment and ask you if you want to switch(Bottom right corner of the screen), click 'Yes'.

![Prompt to switch to the virtual environment](/assets/img/courses/id5415/module1/assignment/2_5_1.png)

Kill the Terminal (little trashcan icon) and open a new Terminal to load this new environment. Notice the difference, the Terminal statement starts with `(venv)` and the Python environment is selected in the bottom panel.

![Virtualenv](/assets/img/courses/id5415/module1/assignment/2_5_2.png)

**What did we achieved?**

- We have a machine ready for developing, executing and sharing Python code.

# Step 3: Set up your connected light bulb

Throughout this course, we will control a connected light bulb to prototype a service similar to the [GoodNight Lamp](http://goodnightlamp.com/). Let's have a look to the light bulb in the prototyping kit: the TP-Link LS110 or LS130.

## Task 3.1 Screwing the light bulb

The 101 step of any light bulb is to screw it on a socket, the LS100 series fit on an E27 socket. Make sure it is powered so that its WiFi is turned on.

## Task 3.2 Installing a Python library

To interact with the light bulb, we will rely on a Python library. A software library is a set of data, a piece of code and documentation. In our case, the [python-kasa](https://python-kasa.readthedocs.io/en/latest/index.html) library will help us interact with the light bulb.

To install this library, go back to VS Code in your course directory and open a Terminal.

We will use pip to install a Python library. Thanks to the [virtualenv setup](#task-25-setting-up-virtual-environment-to-work), we can install this library inside the course folder only, avoiding any disruption of other Python program on your machine. In the following command,the --pre option signal that we want to install the most recent version of the library, including pre-release.

```bash
pip install python-kasa --pre
```

kasa## Task 3.3 Connecting to the Network

To connect the light bulb to the network, we need to provision it with the WiFi network information. Taken out of [the documentation](https://python-kasa.readthedocs.io/en/latest/cli.html#provisioning), here are the steps:

1. You need to turn on and of the light bulb 3 times in a row. The light bulb blinks a couple of times. This is the way to let it disconnect from any network and start emitting its own WiFi network;
2. Connect your personal computer / laptop to the light bulb network (it should appear as TP-LINK*Smart Bulb*...). Make sure that your machine is not connected to a wired network at the same time.
3. Execute the command `kasa discover` to locate the IP address of the device (likely 192.168.0.1)

```bash
kasa discover
```

![IP Address of the light bulb](/assets/img/courses/id5415/module1/assignment/3_3_1.png)

When the light bulb is found, at the top of the result you will note 'Host:' followed by 4 digits separated by dots. This is the IP address of the light bulb on the network, i.e. the address to send messages to it.

- You can scan for the WiFi networks, in case you do not know which one to connect the light bulb to.

```bash
kasa --host <YOUR_HOST_IP> wifi
```

![Scan discoverable WiFi](/assets/img/courses/id5415/module1/assignment/3_3_2.png)

- Finally, we send a message to the light bulb with the WiFi network information. In the following command, the `<HOST>` is the IP address of the light bulb identified in step 3, 'wifi join' are the command and subcommand, `<YOUR_WIFI_SSID>` is the SSID of the network to connect to (i.e. name of the network/Wifi), the password option provision the network/wifi password and `keytype` is the category of the network.

```bash
kasa --host <HOST> wifi join <YOUR_WIFI_SSID> --password <YOUR_WIFI_PASSWORD> --keytype="3"
```

If the process works, the light bulbs blinks a two of time, then connect to the provisioned network.

![Connect Smart-bulb to the home/office wifi](/assets/img/courses/id5415/module1/assignment/3_3_3.png)

NOw, connect your machine back to your home network/wifi and run the discover command. Make sure that your machine is not connected to a wired network at the same time, otherwise the discover function might look on the wrong network and find no device.

```bash
kasa discover
```

![IP Address of the light bulb after connected to local WiFi](/assets/img/courses/id5415/module1/assignment/3_3_4.png)

## Task 3.4 Interacting with the Light Bulb

It is now the time to explore what are the capabilities, using the [documentation](https://python-kasa.readthedocs.io) or the --help option

```bash
kasa --help
```

![IP Address of the light bulb after connected to local WiFi](/assets/img/courses/id5415/module1/assignment/3_4_1.png)

```bash
kasa --host <HOST> brightness
```

```bash
kasa --host <HOST> off
```

```bash
kasa --host <HOST> on
```

![Kasa commands to control the light-bulb from machine](/assets/img/courses/id5415/module1/assignment/3_4_2.png)

**What did we achieved?**

- we are able to control the connected light bulb from our computer

# Step 4: Set up a Raspberry Pi as Home Hub

What is a Raspberry Pi, in contrast with an Arduino-like device? Here is a comparison, opposing a ['Microprocessor' and a 'microcontroller'](https://www.youtube.com/watch?v=7vhvnaWUZjE). Why do we use another computer rather than our own machine? Throughout the course, you will also test your code on your machine. However, when prototyping connected products, you want them to be connected over time, especially for a home hub, and not depending on your laptop activity (e.g. closing the lead, moving out of the house). The Raspberry Pi can be permanently connected and serve its purpose. Besides, it also makes a device on which we can set up network access, enabling your laptop, your phone and other devices to interact with it.

## Task 4.1: Installing the Image

By now your Raspberry Pi image should be ready. Let's go back to [Bucket](https://dwd.tudelft.nl/bucket), navigate to you Thing in the left. If the generation is complete, a blue download button appears. Press it to download the file.

You receive a zip file. Unzip it to obtain an image file (extension .img)

To install this image on the SD card, download and install Etcher: [Etcher](https://www.balena.io/etcher/)

Starting Etcher, you first select your image file, then your SD card, and 'Flash'.

Slide in the SD card into the Raspberry Pi and power it with the USB micro charger. Avoid powering the Raspberry Pi with your laptop as it draws more power than your USB port are designed for.

If you properly entered the details of your home network, your Raspberry Pi should automatically connect to this network. After a couple of minutes, refresh your Thing page on the Bucket web app. You should see the IP address of your Raspberry Pi at the top of the page. We will use the local IP address.

![Connected Thing](/assets/img/courses/id5415/module1/assignment/4_1_0.png)

## Task 4.2: Connecting to the Raspberry Pi

You can connect your Raspberry Pi to a screen, a keyboard and a mouse to use it as you would use your own computer. However, while prototyping your Raspberry Pi is often embedded in your setting and knowing how to handle it remotely is an important skill to have.

Throughout this course, we will thus access the Raspberry Pi remotely. For this we will use the ssh command as follows. Replace the square brackets with the username and hostname that you provisioned on Bucket. Pressing enter, you will be prompt for your password.

```bash
ssh [username]@[hostname]
```

Another way to connect to your Raspberry Pi, less convenient but often more reliable, is via its local IP Address (displayed on the Bucket web app). It is composed of 4 numbers separated by dots.

```bash
ssh [username]@[your.local.IP.address]
```

![SSH Pi using Username & Hostname](/assets/img/courses/id5415/module1/assignment/4_2_0.png)

## Task 4.3: Controlling the Light Bulb from Raspberry-Pi

As a final task for this assignment, let's replicate what you achieved on your laptop controlling the light-bulb. e.g Controlling the smartpyth-bulb from Pi

On the Raspberry Pi, open the terminal and create a test directory with the command `mkdir` and navigate inside it with the command `cd`.

```bashc
mkdir test
cd test
```

![Command Line tool in Pi](/assets/img/courses/id5415/module1/assignment/4_3_0.png)

Now first we will create the virtual env using the same command as we did on our personal machine.

To create a virtual environment, in the terminal type below command to create a virtual environment called `venv`.

```bash
python3 -m virtualenv venv
```

And then activate this virtual environment by:

```bash
source venv/bin/activate
```

![Creating and Activation Virtual Environment](/assets/img/courses/id5415/module1/assignment/4_3_1.png)

Now install the kasa-python library to control our light bulb using Raspberry Pi. Type below command in the terminal with while our newly created virtual environment venv is activated.

```bash
pip install python-kasa --pre
```

![Installing kasa library](/assets/img/courses/id5415/module1/assignment/4_3_2.png)

As we already connected the light bulb to our home/office network, we can skip the network provisioning step this time. However, make sure that the pi is connected to same network/wifi as smart-bulb

Lets use some of the commands from kasa library to control the light bulb:

First to check if the bulb is configured connected to the WiFi network, Turn ON the bulb and type

```bash
kasa discover
```

This will show the name of our smart bulb, its IP address, some hardware details and its current status with device-specific information.

![Running kasa discover command in Pi terminal](/assets/img/courses/id5415/module1/assignment/4_3_3.png)

Now that we have the IP address of the bulb (HOST), we will try to interact with light bulb by running some simple commands from kasa library.

```bash
kasa --host <HOST> brightness
```

```bash
kasa --host <HOST> off
```

```bash
kasa --host <HOST> on
```

![Kasa commands to control the light-bulb from Pi](/assets/img/courses/id5415/module1/assignment/4_3_4.png)

**What did we achieve?**

- we have a Raspberry Pi up and running, connected to the same network as the light-bulb and able to control it.
