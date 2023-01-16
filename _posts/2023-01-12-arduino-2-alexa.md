---
layout: post
title: Control Arduino with Alexa - on premmise infrastructure
subtitle: Each post also has a subtitle
gh-repo: franksheppard/franksheppard.github.io
gh-badge: [star, fork, follow]
tags: [arduino, alexa]
comments: true
published: false
---

A couple of years ago I received as a present an Arduino board and I haven't realy got the chance to play with it.

## Abstract

I want to control an Arduino board with Alexa without 3rd party IOT cloud services.
I will use

## Requirements

To build this project i'll use the follwing:

- Arduino board
- Ethernet/Wifi Arduino Shield
- Home router (the one provided by your ISP(Internet Service Provider) will do just fine)
- Docker host (old PC with Docker Desktop instaleld )
  - mosquitto container
  - Nodered container
- Alexa or Google Home device

To write the neccessary code I'll be using VsCode

## Architecture

The architecture its fairly simple. The Arduino board is connected to the home network via the Arduino Ethernet Shield and routed to the Docker Hosts Mosquitto container. Node red will connect to Alexa and forward the packets to Mosquitto server which will send them to the board. A diagram will show this a lot better.
