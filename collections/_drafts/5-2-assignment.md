---
layout: course-page
title: 'Enriching and Sharing'
permalink: /draft/module5/assignment
description: 'Prototyping Connected Product - Assignment 5'
assignment-id: 5
assignment-of: id5415-5
introduction: In this assignment, you will enriched your lighting system with external web services and open it up for external access.
prog_environment:
design:
code_management:
computational_concepts:
---

## Assignment 5 focuses on implementing a functionlity to interact with the lightbulb from a webpage using a local server. We will first create a a local web-server in our machine. Through this web-server we will create a simple web page with button that interact with the bulb.

---

- Do not remove this line (it will not be displayed)
{:toc}

---


# Step 1 Weather Web Service

## Task 1.1 Open Weather API

!['Open Weather API'](/assets/img/courses/id5415/module5/assignment/1_1.png)

capabilities
current weather

## Task 1.2 Get Current Weather


>It's time to open VS Code and load the virtual environment in the Terminal.

Let's create a new file `weather.py` with the following code.

```python
# Python package to excute an HTTP request
import requests

# Loading environment variables from the .env file
from dotenv import load_dotenv
import os
load_dotenv()

OPEN_WEATHER_API_KEY = os.getenv("OPEN_WEATHER_API_KEY", None)

# the url of the weather API we want to request
url = "https://api.openweathermap.org/data/2.5/weather"
# 
querystring = {"appid": OPEN_WEATHER_API_KEY,"q":"Delft,nl"}
response = requests.request("GET", url, params=querystring)
print(response.text)
```

In this code, we import the `requests` package which we use to execute an HTTP request. We will need first to install the package `request`.

```bash
pip install requests
```

Then, we load the API key from the .env file. For this to work, we need to add our Open Weather API key in the `.env` file.

```bash
OPEN_WEATHER_API_KEY=[REPLACE WITH YOUR KEY]
```

We set the variable `url` with the URL of the weather service, as specified in the [API documentation](https://openweathermap.org/current)

!['API Documentation Current Weather'](/assets/img/courses/id5415/module5/assignment/1_2_1.png)

We can see in the documentation that a few parameters are required: `appid` (our api key) and `q` (location we are interested in). We use the variable `querystring` to put this information in a dictionary. Finally, send the request with the URL and the querystring as 'params'. Not the 'verb' of the HTTP request `GET`, as we want to 'get' data from the web service.

We can now execute the code to get the current weather.

```python
python src/weather.py
```

The result should look like this:

![Raw result of weather data](/assets/img/courses/id5415/module5/assignment/1_2_2.png)

## Task 1.3 Format JSON

The weather web service sends us data as JSON format. We can make the result more readable with the JSON package (to import at the to of the file). We use `json.loads()` to read the response as a JSON structure, then `json.dumps()` to print this structure. Note the `indent` parameter which makes the whole structure more readable by indenting each sub key/value set.

```python
import json

# ...

json_result = json.loads(response.text)
print(json.dumps(json_result, indent=4))
```

## Task 1.4 Request parameters

We can now read through the keys and values of the result. The temperature `temp`, `temp_max` and `temp_min`, we note that the unit is not degrees. The weather report is in English. Looking back at the [API documentation](https://openweathermap.org/current), there are parameters `units` and `lang` to tune this information. You can add these option to your `querystring` dictionary and observe the change in the request result.

!['API Documentation Parameters'](/assets/img/courses/id5415/module5/assignment/1_4.png)

## Task 1.5 Extract values

Similarly to the lightbulb result in Module 2, we need to extract values from this structure to use them as a control of the lightbulb.

Extracting the cloudiness would look like this, first reading the value of the key 'clouds', then inside this value, looking for the key 'all'.

```python
cloudiness = json_result["clouds"]["all"]
print(cloudiness)
```

## Task 1.6 Control the lightbulb

You can use this approach to extract values out of the weather result and control the lightbulb. For example, these values could be used for controlling the brightness or the number/duration of the blink function.

# Step 2 Web Server

In the first step, we consumed a web service by sending an HTTP request to the Open Weather server and parsing the result. In this step we will offer our own web service by running a web server.

# Step 1: Configuring Webserver

## Task 1.1 Installing Flask Web-server

To create the web-server, we will use a python web-server model called [Flask](https://palletsprojects.com/p/flask/).

Like other Python library, you can install this web-server module using pip. In your VS Code terminal activate the virtual environment and type in:

```bash
python -m pip install Flask
```

## Task 1.2 Configuring the REST API

TODO A little bit information about what is REST API

Now that we have webserver module installed, let's implement a simple REST API.

First create a new folder `web` inside `src` folder of your project. Then, create a file `server.py` and insert the following line of python code:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run()
```

This code import the flask library and create an `app`, a typical name to refer to the web application you start building.

The line starting with `@` is an annotation, used to complement the code. In this context, we specify a web path, the root '/', to trigger the function `hello_world()` right bellow.

Finally, the script define the `main`, where the programme start. In this case `run` our `app`, meaning we start the web server.

In the terminal, execute the Python file to start the web server:

```bash
python3 web/server.py
```

You will see that server has been started.

![Flask Server Start](/assets/img/courses/id5415/module5/assignment/flask_start.png)

Open a web browser and type in: http://localhost:5000/

You should see appear a 'Hello world' on the web page. This is the result of the hello_world function. In VS Code, in the `hello_world` function, you can change the message. Then, in the terminal, stop the server (Ctrl+C) and execute your script again (Arrow up + Enter). Back in the web browser, refreshing the page should display your new message.

## Task 1.3 Create Simple webpage

So far we have written a piec of code that is running on the server side and directly printing a `Hello World` message. NowLet's try to create a simple webpage with button that print the `hello world` message

first inside the `web` folder, create another two folders called `static` and `templates`. Inside templates, create an html file called `index.html`.

Copy the following line of HTML code inside the file:

TODO clicking button should print light bulb details

```html
<html>
  <head>
    <title>My lightbulb app</title>

    <script
      src="//cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js"
      integrity="sha256-yr4fRk/GU1ehYJPAs8P4JlTgu0Hdsp4ZKrx8bDEDC3I="
      crossorigin="anonymous"
    ></script>
    <script type="text/javascript" charset="utf-8">
      var socket = io();
      socket.on('connect', function () {
        // socket.emit('json', {data: 'I\'m connected!'});
      });

      socket.on('json', function (msg, cb) {
        if (msg.data !== undefined) {
          gauge1.update(msg.data);
        }
      });

      function sendMessage(message) {
        socket.emit('json', { data: NewValue() });
      }
    </script>
  </head>
  <body>
    <h1>My lighbulb App</h1>
    <button onclick="sendMessage('Hello World!')">Click!</button>
  </body>
</html>
```

Now go to `server.py` and add replace:

```python
@app.route('/')
def hello_world():
    return 'Hello, World!'
```

with the following function:

```python
@app.route('/')
def home():
    return render_template('index.html')
```

Restart the server by running `server.py` again and refresh the page on your browser. You will see a that the webpage we have created with title and button has been rendered in our brwoser.

## Task 1.4 External Access of the server

For now, this web page is only accessible from your computer. In your `server.py`, at the bottom, you can provide a parameter `host='0.0.0.0` to the run function to remove this constraint:

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

This means that your server will accept incoming request from any source e.g mobile, tablet connected to the same network. However you need to know the ip address of your current machine that is hosting this server.

You can look up your ip address with the command

**Mac/Linux**

```bash
ipconfig getifaddr en0`
```

**Windows**

Type below command in command prompt and look for `IPv4 Address. . . . . . . . . . . : 192.*.*.*`

```bash
ipconfig
```

Once you have the local ip address of your machine, use a phone or other computer connected and type this local ip address of your machine. You will see a webpage with title and button has been rendered.

**Note:** On some platform you might also need to disable/create a rule for your Firewall.

For information, [Fullstack](https://www.fullstackpython.com/flask.html) provides some useful information to go further.

# Step 2: Code a REST API

TODO Share sensor data, control light bulb

So far we have created REST Api as the default action that will print the hello world message. However, a RESTFul API can include more than that. Here is an informative link: [REST API](https://www.restapitutorial.com).

TODO Code for therest api in our light-bulb context

TODO function to retrieve the lightbulb details when click on html button
