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

# Building the hardware

As a base for all the modules I used ESP 8266 and ESP32 micro-controllers were used, I picked those models because of their low cost, good performance and Wi-Fi capabilities. 

To make developing the software easier I chose to use Vscode as my IDE alongside the [PlatformIO](https://github.com/platformio) extension. 
This extension makes writing and deploying code to the micro-controllers a breeze since it allows the use of Vscode great code completions and it also features a library manager that allows the easy inclusion of external libraries.
This also allows the compilation and uploading of code to the micro-controllers via CLI (this will come in handy in the future).

## Light control

To reduce the number of micro-controllers needed I decided to run all the lights to a central module, this module used a [Wemos D1 Mini](https://www.wemos.cc/en/latest/d1/d1_mini.html) as the micro-controller paired with mosfets to allow the use of high power LEDs. 
Below you can find the module schematic as well as a fully assembled protoboard (excuse the not-so professional soldering job 🤪).
![Light control PCB](/assets/img/posts/CourseFinalProject/Device HUB_bb.png)
*Module Schematic*

|                         Top View                          |                         Bottom View                         |
|:---------------------------------------------------------:|:-----------------------------------------------------------:|
| ![](/assets/img/posts/CourseFinalProject/DeviceHubUP.jpg) | ![](/assets/img/posts/CourseFinalProject/DeviceHubDown.jpg) |


