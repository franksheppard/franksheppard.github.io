---
layout: post
title: How To Use Nmap to Scan for Open Ports on your VPS
subtitle: 
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [python, mac]
published: true
---
## Introduction

Networking is an expansive and overwhelming topic for many budding system administrators. There are various layers, protocols, and interfaces, and many tools and utilities that must be mastered to understand them.

This guide will cover the concept of "ports" and will demonstrate how the nmap program can be used to get information about the state of a machine's ports on a network.

Note: This tutorial covers IPv4 security. In Linux, IPv6 security is maintained separately from IPv4. For example, "nmap" scans IPv4 addresses by default but can also scan IPv6 addresses if the proper option is specified (nmap -6).

If your VPS is configured for IPv6, please remember to secure both your IPv4 and IPv6 network interfaces with the appropriate tools. For more information about IPv6 tools, refer to this guide: How To Configure Tools to Use IPv6 on a Linux VPS

What Are Ports? There are many layers in the OSI networking model. The transport layer is the layer primarily concerned with the communication between different services and applications.

This layer is the main layer that ports are associated with.

## Port Terminology

Some knowledge of terminology is needed to understand port configuration. Here are some terms that will help you understand the discussion that will follow:

Port:
An addressable network location implemented inside of the operating system that helps distinguish traffic destined for different applications or services. 

Internet Sockets: A file descriptor that specifies an IP address and an associated port number, as well as the transfer protocol that will be used to handle the data. 

Binding: The process that takes place when an application or service uses an internet socket to handle the data it is inputting and outputting. 

Listening: A service is said to be "listening" on a port when it is binding to a port/protocol/IP address combination in order to wait for requests from clients of the service.
Upon receiving a request, it then establishes a connection with the client (when appropriate) using the same port it has been listening on. Because the internet sockets used are associated with a specific client IP address, this does not prevent the server from listening for and serving requests to other clients simultaneously.

Port Scanning: Port scanning is the process of attempting to connect to a number of sequential ports, for the purpose of acquiring information about which are open and what services and operating system are behind them. Common Ports Ports are specified by a number ranging from 1 to 65535.
Many ports below 1024 are associated with services that Linux and Unix-like operating systems consider critical to essential network functions, so you must have root privileges to assign services to them. Ports between 1024 and 49151 are considered "registered". This means that they can be "reserved" (in a very loose sense of the word) for certain services by issuing a request to the IANA (Internet Assigned Numbers Authority). They are not strictly enforced, but they can give a clue as to the possible services running on a certain port. Ports between 49152 and 65535 cannot be registered and are suggested for private use. Because of the vast number of available ports, you won't ever have to be concerned with the majority of the services that tend to bind to specific ports. 

However, there are some ports that are worth knowing due to their ubiquity. The following is only a very incomplete list:
- 20: FTP data 
- 21: FTP control port 
- 22: SSH 
- 23: Telnet <= Insecure, not recommended for most uses 
- 25: SMTP 
- 43: WHOIS protocol 
- 53: DNS services 
- 67: DHCP server port 
- 68: DHCP client port 
- 80: HTTP traffic <= Normal web traffic 
- 110: POP3 mail port 
- 113: Ident authentication services on IRC networks 
- 143: IMAP mail port 
- 161: SNMP 
- 194: IRC 
- 389: LDAP port 
- 443: HTTPS <= Secure web traffic 
- 587: SMTP <= message submission port 
- 631: CUPS printing daemon port 
- 666: DOOM <= This legacy FPS game actually has its own special port 
 
These are just a few of the services commonly associated with ports. You should be able to find the appropriate ports for the applications you are trying to configure within their respective documentation.


Most services can be configured to use ports other than the default, but you must ensure that both the client and server are configured to use a non-standard port.

You can get a short list of some common ports by typing:

```bash
less /etc/services 
```

It will give you a list of common ports and their associated services:
```bash
. . . tcpmux 1/tcp # TCP port service multiplexer echo 7/tcp echo 7/udp discard 9/tcp sink null discard 9/udp sink null systat 11/tcp users daytime 13/tcp daytime 13/udp netstat 15/tcp qotd 17/tcp quote msp 18/tcp # message send protocol . . . 
````

We will see in the section about nmap how to get a more complete list.

How To Check Your Own Open Ports There are a number of tools that can be used to scan for open ports.

One that is installed by default on most Linux distributions is netstat.

You can quickly discover which services you are running by issuing the command with the following parameters:
```bash
sudo netstat -plunt Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 785/sshd

tcp6 0 0 :::22 :::* LISTEN 785/sshd This shows the port and listening socket associated with the service and lists both UDP and TCP protocols.
```
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