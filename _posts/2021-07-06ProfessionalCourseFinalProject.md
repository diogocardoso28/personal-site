---
title: Aria - Your Smart Home Platform
author: diogo
description: An overview of my professional course (High School) final year's project
date: 2021-07-06 12:00:00 +0800
categories: [Projects]
tags: [smart home, technology, arduino, micro-controllers]
---

# Project Backstory

Around january 2021 I started to get interested in home automation systems, which led me to doing research on platforms like [home assistant](https://www.home-assistant.io/) and [hubitat](https://hubitat.com/).

One of the requirements to finish a professional course in Portugal is the development of a final project, this project must be developed in the final year in roughly three months.
Once was time to pick a topic I immediately remembered my research about the previously mentioned home automation platforms, I immediately thought 'can I build something like this?'

Fortunately, one of the topics on the list was smart home systems, and I promptly chose it.

# Requirements

Although we had freedom to use whatever tech stack we pleased, the school set the following requirements:

1. If a web-based front end was used, it had to use PHP
2. A relational database must be the primary method of providing persistence. (preferably [MySql](https://www.mysql.com/))
3. The database must contain at least three tables and all types of relationships.

# Objectives

After looking at the features of existing home automation systems, I made a list of features that I wanted to implement:
1. Light control
2. Air conditioning system
3. Gas leakage sensing
4. Web radio streaming
5. Video doorbell system
6. Remote garage door opening
7. Control over voice assistant

# Building the Modules

As a base for all the modules I used ESP 8266 and ESP32 micro-controllers were used, I picked those models because of their low cost, good performance and Wi-Fi capabilities. 

Since all modules required WiFi connectivity and communications trough MQTT I decided to make a base firmware file that handled this common behavior, this common file would then be extended my module specific control libraries. This base library also provided a web UI which could be used to change WiFi and MQTT credentials. 

To make developing the software easier I chose to use Vscode as my IDE alongside the [PlatformIO](https://github.com/platformio) extension. 
This extension makes writing and deploying code to the micro-controllers a breeze since it allows the use of Vscode great code completions and it features a library manager that allows the easy inclusion of external libraries.
This also allows the compilation and uploading of code to the micro-controllers via CLI (this will come in handy in the future).

## Light control

To reduce the number of micro-controllers needed I decided to run all the lights to a central module, this module used a [Wemos D1 Mini](https://www.wemos.cc/en/latest/d1/d1_mini.html) as the micro-controller paired with mosfets to allow the use of high power LEDs. 
Below you can find the module schematic as well as a fully assembled protoboard (excuse the not-so professional soldering job 🤪).

[Light control PCB](/assets/img/posts/CourseFinalProject/Device HUB_bb.png)
*Module Schematic*

|                         Top View                          |                         Bottom View                         |
| :-------------------------------------------------------: | :---------------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/DeviceHubUP.jpg) | ![](/assets/img/posts/CourseFinalProject/DeviceHubDown.jpg) |

## Air conditioning system

In order to simulate the air conditioning system two fans and a temperature and humidity sensor were used. The sensor chosen was an BMP 180 due to it's good response time, accuracy and low cost. The two previously mentioned fans were used to simulate the airflow on the indoor/outdoor ac units, to give better realism this fans were inclosed on 3D printed enclosures that looked like AC indoor/outdoor units. This module also allowed the display of current temperature and humidity readings on the UI.


|                  Enclosure 3D View                   |                  Module Schematic                   |
| :--------------------------------------------------: | :-------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/ac_cad.png) | ![](/assets/img/posts/CourseFinalProject/Ac_bb.png) |


|            Temperature and humidity sensor            |                  Indoor/outdoor units                   |
| :---------------------------------------------------: | :-----------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/ac_bmp.jpeg) | ![](/assets/img/posts/CourseFinalProject/ac_units.jpeg) |


The change of desired temperature in a room is made through an intuitive click on a temperature gauge on the user interface (UI).

![Ac UI](/assets/img/posts/CourseFinalProject/ac_temp.png)
*User interface where temperature can be changed*

## Gas leakage sensing

One of the most important things a home can (and should have) is a gas leakage sensor (if gas is used) in order to detect and warn you about a gas leak. In order to achieve this a MQ-5 gas sensor was used in conjunction with a buzzer to provide an audible warning. The sensor was places inside a 3D printed kitchen cabinet. An event was also published via MQTT so that a warning could be displayed on the UI, this event could also be used in the future to add future actions.

|                        Gas sensor                         |                      Protoboard                       |
| :-------------------------------------------------------: | :---------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/gas_sensor.jpeg) | ![](/assets/img/posts/CourseFinalProject/gas_pcb.jpg) |

## Web radio streaming

Another feature that's nice to have in a smart home is a central radio system, to achieve this a Vs1053 MP3 decoder board in conjunction with an esp32 micro controller. A small speaker was placed in a 3D printed enclosure to provide the sound. The target radio station (MP3 stream) and volume controls where provided via MQTT messages.

|                    Enclosure 3D View                    |                                                              Protoboard                                                               |
| :-----------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/radio_cad.png) | ![](/assets/img/posts/CourseFinalProject/radio_speaker.jpeg){:width="45%" style="display:block; margin-left:auto; margin-right:auto"} |

![Ac UI](/assets/img/posts/CourseFinalProject/radio_bb.png){:width="50%" style="display:block; margin-left:auto; margin-right:auto"}
*Module Schematic*

## Video doorbell system
This was one of the most challenging parts of the project since it required lots of new techniques both in the hardware and software side. The micro controller chosen was a esp32 cam, OLED display, photoresistor and high power LED (built in to micro-controller) for night time illumination. The OLED screen was used to display the current time and inform the person ringing it if it was on do-not disturb mode or not. If the do not disturb mode was enabled no sound would be produced inside of the home. This module would send an MQTT message each time the doorbell was rang. To provide real time video on the dashboard [FFmpeg](https://www.ffmpeg.org/) was used to re stream via HTTPS the local video from the ESP. 

|                       Exploded 3D View                       |                       Assembled 3D view                       |
| :----------------------------------------------------------: | :-----------------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/cam32_exploded.png) | ![](/assets/img/posts/CourseFinalProject/cam32_assembled.png) |

## Remote garage door opening

In order to simulate the opening and closing of a gate a 3D printed gate was used, this assembly featured and electric motor to move the gate and two end-stops to provide feedback of the gate's state (in case it was moved my hand). To view and control the gate a card was shown on the UI.

|                        3D View                         |                                                        Module Schematic                                                        |
| :----------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/gate_cad.png) | ![](/assets/img/posts/CourseFinalProject/gate_bb.png){:width="50%" style="display:block; margin-left:auto; margin-right:auto"} |

To control the direction of the DC motor two relays were used, these relays would invert the polarity of the current running trough the motor which changed the gate's direction. 
The gate was attached to a threaded rod and the motion was transferred via a nut inclosed in a gear.
Below you can find a close up look at previously the mentioned system.

![Gate motion system close-up](/assets/img/posts/CourseFinalProject/gate_cad_motion.png)
*Gate motion system close-up*

## Control over voice assistant

In order to provide this project with voice control google's [dialogflow](https://dialogflow.cloud.google.com/) to provide a custom google assistant integration. Since this was an academic project only a few interactions (intents) were developed. 'getTemperatureIntent' was used to obtain the temperature in a specified room, 'turnLightOffIntent' and 'turnLightOnIntent' were used to control lights. Below you can find a listing of all the created intents and example phrases.

|                        DialogFlow Intents                        |                 'getTemperatureIntent' Phrases                  |
| :--------------------------------------------------------------: | :-------------------------------------------------------------: |
| ![](/assets/img/posts/CourseFinalProject/dialogflow_intents.png) | ![](/assets/img/posts/CourseFinalProject/dialogflow_phrase.png) |

The highlighted word is the placeholder for the variable, in this case the name of the room.


In order to facilitate the development of the software that would handle the requests coming from google assistant a framework was used. [Jovo](https://www.jovo.tech/) is an open source framework  that allows easier development of apps to run on voice assistants. This framework ran a [node js](https://nodejs.org/) webserver that gathered the data from the smart home's api and provided responses to google assistant. To be able to provide a secure url to dialogflow's actions an external HTTPS connection was needed, which required port-forwarding. The final presentation had to be performed on a network that didn't allow port forwarding to the exterior.he final presentation had to be performed on a network that didn't allow port forwarding to the exterior.
Since the project's final presentation was made in a network that didn't allow port-forwarding [ngrok](https://ngrok.com/) was used to create an secure tunnel.

![Google assistant screenshot](/assets/img/posts/CourseFinalProject/dialogflow_assistant.png)
*Google assistant screenshot*


## The backend

To build this project's backend [node js](https://nodejs.org/) and [express](https://expressjs.com/) were used. As previously mentioned the database implemented had to be MySql and it had the following tables:

![Database diagram](/assets/img/posts/CourseFinalProject/database.png)
*Database diagram*


The 'home_device' table contains information about devices (such as light bulbs, power sockets, and gates) capable of performing actions. The 'home_device_value' table logs state changes of these devices and registers the user if the change is made through the smart home platform. 'Home_device_type' and 'sensor_type' tables maintain records of all existing device and sensor types along with their respective firmware files. 'config_table' stores the basic configurations to be uploaded to the sensors/devices.

An api was also created to allow the development of external applications. The previously mentioned google assistant integration was built using this api. 

### Allowing run-time addition of sensors and actuators

To give the user the ability to add new sensors/actuators and not have to manually upload firmware to the ESPs, a function was added that after the insertion of the device trough the UI it would request the user to plug the device into the computer running the backend (in this case a raspberry pi). After the user confirmed the device was plugged in, the node would launch a shell command calling [PlatformIO](https://github.com/platformio) to upload via CLI. Before uploading the code the backend would also update the config files accordingly.

# A house to the hardware

To finalize this effort a miniature house was made to house all the components.

![Miniature House](/assets/img/posts/CourseFinalProject/fundo.jpeg)

# A look back on the project

Looking back at this project I had a ton of fun putting this all together. This was an awesome learning experience as I got the mix the things that I love doing the most, building hardware, writing software and 3D printing. All the issues found and solutions created made me a better developer (in my opinion).