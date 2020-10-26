---
layout: course-page
title: "APIs and the Web"
permalink: /module5/self-study
description: "Prototyping Connected Product - Self-Study 5"
self-study-id: 5
self-study-of: id5415-5
tags:
- API
- Web Services
- HTTP
- REST
introduction: In this module, the self-study material focus on web technology. We will explore web services and how they leverage the world wide web. Diving in the technology, we will introduce HTTP and the REST APIs.
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---

# API

<span class="mdi mdi-text-box-outline"></span> Reading (10 minutes)

For a connected device, the way it __connects__ and interacts with other devices is fundamental. Its specification falls into the responsibility of the designer, as it exposes functionalities of the connected product.

This exposure is called `API` (for Application Programming Interface). An API defines how other devices can interact with our device through code. It is a machine-to-machine interaction. These interfaces exist throughout the code we write. It is an abstraction: as users of an API, we only use what is exposed. An example of APIs we already used in this course are functions. Other parts of the code use our Python function such as `blink()` or `save_in_file()`. They expose functionalities of our software controlled by code, e.g. __Programming Interface__. At the same time, they hide all details of how they achieve the functionalities: to use a function we do not need to know how it is implemented.

Across devices, APIs expose services of our connected product over the network. In this context, APIs also bridge the gap between software implemented in different languages. For example, our Python code can interact with Bucket, which is implemented in `JavaScript` language. It can also interact with the connected lightbulb without knowing which programming language it uses.

APIs define what functionalities of our connected product are exposed and how. Their specification falls into the responsibility of the designer.

# Web services

<span class="mdi mdi-text-box-outline"></span> Reading (10 minutes)

`The Internet` is the global network of machines as we described it in the previous Module. This Internet Protocol (IP) and its underlying infrastructure allows all applications we know to interact globally, from web browsing to email, video streaming or instant messaging. In contrast, the World Wide Web or `web` is one way of exchanging information over the Internet. It is the way our web browser is retrieving web pages.

The web relies on the Hypertext Transfer Protocol (`HTTP`). What makes a text `hyper`? Any text meant to be displayed on an electronic device with links to other content: forming a web of information. A fundamental element of HTTP is the system of `URL` (for Uniform Resource Locator), the protocol that defines how they link to further hypertexts. As an example, at the top of your web browser, you can see the URL of the current page of this course:

`https://id5415.datacentricdesign.org:443/module5/self-study`

* `https` (or http) is the protocol we use, the 's' using a 'secured', encrypted protocol over HTTP
* `id5415.datacentricdesign.org` is the host, the name replacing the IP address of the machine running the webserver to make it easier and fix
* `443` is the port on the machine running the webserver, probably hidden by your web browser as it is the default port for `https`
* `/module5/self-study` is the path to the resource

How does it relate to connected products? The web turned out to be a powerful way to link resources over the Internet. A `web service` is leveraging this approach, like our web browser load web pages, to expose a set of APIs offered over the `web`. Web pages and web services rely on the same protocols and technologies. The only difference is that the page is meant to be consumed by humans and the services are meant to be consumed by machines.

**Note**: In this course, we focus on APIs exposed through web services, thus across the web. However, there are APIs defined on each device that talks to other devices. You might want to look at GATT services are if you are interested in the interaction between Bluetooth devices.

# HTTP Request

<span class="mdi mdi-text-box-outline"></span> Reading (10 minutes)

So far, we saw that we could rely on the web to exchange information between connected products in the same wait we browser web pages. However, what does that mean? To request a web page or service, we send an HTTP request. Following the example of the current page, the HTTP request looks as follows:

```
GET https://id5415.datacentricdesign.org/module5/self-study HTTP/2
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:81.0) Gecko/20100101 Firefox/81.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
```

The first line starts with the HTTP methods `GET`, meaning that we ask for content from the server. It is followed by the URL and the version of the HTTP protocol (2).

The following lines are `headers`, complementary information about the HTTP request. This example is from Firefox on a MAC, using the 'User-agent' header to tell the server who it is. The 'Accept' headers specify the server how we want to receive the information. Headers could also include authentication information.

An HTTP request can also contain a `message-body`, information that we would want to send to the server as part of the request such as a JSON structure with our lightbulb data. In this case, the header 'Content-Type' would specify what we are sending to the server.

# REST API

<span class="mdi mdi-text-box-outline"></span> Reading (45 minutes)

Let's now close the loop. We know what is an API, and we have an overview of how a web service can use the same principle as web pages to enable communication between machines. A REST API brings a set of principles on top of HTTP requests to specify the APIs of your web service. Have a look at [https://www.restapitutorial.com](https://www.restapitutorial.com) to explore these principles.


# Quiz

Check your understanding with the following quiz! It is anonymous and you can try as many times as you want!

<iframe width="640px" height= "600px" src= "https://forms.microsoft.com/Pages/ResponsePage.aspx?id=TVJuCSlpMECM04q0LeCIe-EN8Fz6eUZIqbayPT_HeNhUN1hCQllTTkNQRDFVM1czWUtFRlZISk9QNC4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>