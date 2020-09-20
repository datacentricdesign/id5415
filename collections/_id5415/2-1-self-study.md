---
layout: course-page
title: 'Code and collaboration'
permalink: /module2/self-study
description: 'Prototyping Connected Product - Self-Study 2'
self-study-id: 2
self-study-of: id5415-2
tags:
- Python
- Git
- GitHub
introduction: In this module, the self-study material focus on the fundamentals of programming. While this course is not a programming course, we will explore the necessary basics to get started such as state and code management. We will motivate the choice of Python for this course, and introduce a few specifics to Python. Finally, we will introduce the concept of Version Control Systems and code library, necessary step to use code from others and collaborate.
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---

# Python

<span class="mdi mdi-text-box-outline"></span> Reading (30 minutes)

In the first assignment, we installed Python to use the Python package `python-kasa` to interact with the light bulb of our prototyping kit. Let's go back a few steps.

## What is Python?

>Python is an `interpreted`, `high-level` and `general-purpose` programming language ([Wikipedia](https://en.wikipedia.org/wiki/Python_%28programming_language%29)).

Python is a programming language. Similarly to a natural language such as English, it defines a set of rules on how to tell machines what we want them to do.

* `Interpreted` means that a machine will read our Python code and understand is without the need to transform it. Other languages such as the `C` language require your code to be `compiled` so that the machine can understand it. You have encountered such a step if you programmed with Arduino.
* `High-level` means that you do not have to care too much about the machine that runs your code. In this context, low-level is closer to the `0` and `1` understood by the machine, while `high-level` is closer to humans' natural language.
* `General-purpose` means that Python is not designed for a particular application or domain. We can use it for any type of programming tasks.

## Why do we use Python?

Because it was [invented by the Dutch Guido van Rossum](https://www.youtube.com/watch?v=J0Aq44Pze-w). No, well yes, it is a Dutch design but this is not the reason we use it in this course! Its relevance comes from a global community across disciplines (from mathematics to data science, artificial intelligence and automation) and its acknowledge ease to get started in contrast with other programming languages. These characteristics help us understand the relevance of Python in this course. To prototype a connected product, we need

- a programming language that we can use for a wide variety of tasks from controlling a light bulb to visualising sensor data and fetching the next weather forecast from a web service;
- to get quickly up to speed and focus on the prototyping and testing, rather than the coding itself;
- basic programming skills that we can reuse in many projects, even those that are not relying on Python.


## Store and Manipulate Information

In programming, what we want to achieve is to `store` and `manipulate` information. We aim to provide you with a dry overview of the fundamental programming concepts so that we can quickly switch to hands-on practice with a shared understanding.

In programming, a `variable` is a storage location paired with an associated symbolic name. Each of the variables holds a value that either static or that varies throughout the program.

![Variable](/assets/img/courses/id5415/module2/variable.svg)

What information can we store? We can store any information. However, it needs to be of a certain `data types`. Two common examples are `numerical` (integers, floats, complex) and `textual` information. In programming, we refer to textual information as `strings`, a short for strings (or sequences) of characters. In contrast with other languages, you will notice that in Python you do not specify data types. Python infers automatically the type of information.

So why do types matter if Python takes care of it? Depending on the type of information we deal with, we will not do the same thing. For instance, with `numerical` information we want to make mathematical operations. In contrast, with `textual` information we want to put them together to form sentences.

Storing, comparing, showing on the screen, combining: these are all examples of information manipulation. In programming, we refer to `statement` for each line of code that manipulates information as it influences or relies on the state of a variable (i.e. stored information). Throughout the course, we will introduce statements with the Python syntax. You will recognise two types of statements.

- simple statement fitting on a single line. For example: `a = b + c`
- compound statement spreading over several lines. For example:

```
if light is off
then turn on the light
else turn off the light
```

Notice: the examples above are not written Python. They describe the steps of an algorithm with plain language. This is called `pseudocode`.

Finally, what is not a statement in your code is a `comment`: information for the reader of your code which is ignored by the machine. It is critical for others to understand why you wrote your code in this way, but also for yourself when you come back to your code.

# Version Control System

<span class="mdi mdi-text-box-outline"></span>
<span class="mdi mdi-video"></span>
Reading and watching (45 minutes)

Prototyping connected products introduces a set of development challenges. The purpose of learning the basics of programming in this context is to help you collaborate on the design __and__ development of a wide range of connected products. In fact, the design and development of these products is becoming increasingly agile: a never-ending set of iteration mixing design, implementation and evaluation that requires a common understanding for effective team collaboration. For instance,

- people work in parallel on multiple aspects of the product (UI, network, data, hardware);
- code runs on multiple devices, if only on machines of the team members. In our case it also involves a Raspberry Pi for each team member;
- experiments take place without fear of losing what has been achieved so far;
- test and evaluation can be performed to across multiple versions of a product (or prototype);
- all team members can take action when they identify an issue with the product, without relying on someone else.

These are some of the motivation to introduce a Version Control System (VCS). In this course, we will focus on Git, which you already installed in the first module.

>Disclaimer: it has been scientifically proven that the mental model of git reflects the system's perspective rather than the user's perspective. A bad thing to do, isn't it? Pardon the poor computer scientists and please take on the task of making it better!

In the following video, Alice Bartlett gives us an 'as human as possible' introduction ride through the main concepts of Git.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/eWxxfttcMts" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- `repository`: The directory that contains all the files of your project. A full copy lies on every team members' machine.
- `commit`: a snapshot of your repository

- `hash`: the id attached to each commit, enabling us to uniquely identify each version;
- `checkout`: time travel to a specific commit. At any time, it enables you to bring back to life a version of your code;

- `branch`: a movable label that points to a commit
- `merge`: combining two branches 

- `remote`: a computer with the repository on it
- `clone`: get the repository from the remote for the first time
- `push`: send commits to the remote
- `pull`: get commits from the remote

The following video is illustrating the purpose of GitHub, a web service offering a Git `remote` as well as a set of collaborative and social features.

<iframe width="560" height="400" src="https://www.youtube.com/embed/w3jLJU7DT5E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>