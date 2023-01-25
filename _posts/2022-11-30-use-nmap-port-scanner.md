---
layout: post
title: How to use nmap to scan open ports
subtitle: 
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [python, mac]
published: true
---
## Introduction

Networking is an expansive and overwhelming topic for many budding system administrators. There are various layers, protocols, and interfaces, and many tools and utilities that must be mastered to understand them.

This guide will demonstrate how the nmap program can be used to get information about the state of a machine's ports on a network.

## Install Nmap

Part of securing a network involves doing vulnerability testing. This means trying to infiltrate your network and discover weaknesses in the same way that an attacker might.

Out of all of the available tools for this, nmap is perhaps the most common and powerful.

You can install nmap on an Ubuntu or Debian machine by entering:

```bash
sudo apt-get update 
sudo apt-get install nmap 
```

One of the side benefits of installing this software is an improved port mapping file.

You can see a much more extensive association between ports and services by looking in this file:

```bash
less /usr/share/nmap/nmap-services 
. . . tcpmux 1/tcp 0.001995 # TCP Port Service Multiplexer [rfc-1078] tcpmux 1/udp 0.001236 # TCP Port Service Multiplexer compressnet 2/tcp 0.000013 # Management Utility compressnet 2/udp 0.001845 # Management Utility compressnet 3/tcp 0.001242 # Compression Process compressnet 3/udp 0.001532 # Compression Process unknown 4/tcp 0.000477 rje 5/udp 0.000593 # Remote Job Entry unknown 6/tcp 0.000502 echo 7/tcp 0.004855 echo 7/udp 0.024679 echo 7/sctp 0.000000 . . .
```

Besides having almost 20 thousand lines, this file also has additional fields, such as the third column, which lists the open frequency of that port as discovered during research scans on the internet.

How To Scan Ports with Nmap Nmap can reveal a lot of information about a host. It can also make system administrators of the target system think that someone has malicious intent. For this reason, only test it on servers that you own or in situations where you've notified the owners.

The nmap creators actually provide a test server located at:

`scanme.nmap.org` This, or your own VPS instances are good targets for practicing nmap.

Here are some common operations that can be performed with nmap. We will run them all with sudo privileges to avoid returning partial results for some queries. Some commands may take a long while to complete:

Scan for the host operating system:

`sudo nmap -O remote_host`

Skip network discovery portion and assume the host is online. This is useful if you get a reply that says "Note: Host seems down" in your other tests. Add this to the other options:

Specify a range with "-" or "/24" to scan a number of hosts at once:

`sudo nmap -PN remote_host`

 Scan a network range for available services:

`sudo nmap -PN xxx.xxx.xxx.xxx-yyy`

 Scan without preforming a reverse DNS lookup on the IP address specified.

`sudo nmap -sP network_address_range`

This should speed up your results in most cases:

`sudo nmap -n remote_host`

Scan a specific port instead of all common ports:

`sudo nmap -p port_number remote_host`

 To scan for TCP connections, nmap can perform a 3-way handshake (explained below), with the targeted port. Execute it like this:

` sudo nmap -sT remote_host`

To scan for UDP connections, type:

`sudo nmap -sU remote_host`

Scan for every TCP and UDP open port:

`sudo nmap -n -PN -sT -sU -p- remote_host A TCP "SYN"` scan exploits the way that TCP establishes a connection.

To start a TCP connection, the requesting end sends a "synchronize request" packet to the server. The server then sends a "synchronize acknowledgment" packet back. The original sender then sends back an "acknowledgment" packet back to the server, and a connection is established.

A "SYN" scan, however, drops the connection when the first packet is returned from the server. This is called a "half-open" scan and used to be promoted as a way to surreptitiously scan for ports, since the application associated with that port would not receive the traffic, because the connection is never completed.

This is no longer considered stealthy with the adoption of more advanced firewalls and the flagging of incomplete SYN request in many configurations.

To perform a SYN scan, execute:

sudo nmap -sS remote_host A more stealthy approach is sending invalid TCP headers, which, if the host conforms to the TCP specifications, should send a packet back if that port is closed. This will work on non-Windows based servers.

You can use the "-sF", "-sX", or "-sN" flags. They all will produce the response we are looking for:

sudo nmap -PN -p port_number -sN remote_host To see what version of a service is running on the host, you can try this command. It tries to determine the service and version by testing different responses from the server:

sudo nmap -PN -p port_number -sV remote_host There are many other command combinations that you can use, but this should get you started on exploring your networking vulnerabilities.

Conclusion Understanding port configuration and how to discover what the attack vectors are on your server is only one step to securing your information and your VPS. It is an essentail skill, however.

Discovering which ports are open and what information can be obtained from the services accepting connections on those ports gives you the information that you need to lock down your server. Any extraneous information leaked out of your machine can be used by a malicious user to try to exploit known vulnerabilities or develop new ones. The less they can figure out, the better.