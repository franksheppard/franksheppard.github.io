---
layout: post
title: Networking 101 - What are ports
subtitle: 
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [networking, linux]
published: true
---

Networking is an expansive and overwhelming topic for many budding system administrators. There are various layers, protocols, and interfaces, and many tools and utilities that must be mastered to understand them.

This guide will cover the concept of "ports" and will demonstrate how the nmap program can be used to get information about the state of a machine's ports on a network.

Note: This tutorial covers IPv4 security. In Linux, IPv6 security is maintained separately from IPv4. For example, "nmap" scans IPv4 addresses by default but can also scan IPv6 addresses if the proper option is specified (nmap -6).

If your server is configured for IPv6, please remember to secure both your IPv4 and IPv6 network interfaces with the appropriate tools. For more information about IPv6 tools, refer to this guide: How To Configure Tools to Use IPv6 on a Linux server

## What Are Ports?

There are many layers in the OSI networking model. The transport layer is the layer primarily concerned with the communication between different services and applications. This layer is the main layer that ports are associated with.

## Terminology

Some knowledge of terminology is needed to understand port configuration. Here are some terms that will help you understand the discussion that will follow:

- **Port:**
    An addressable network location implemented inside of the operating system that helps distinguish traffic destined for different applications or services.

- **Internet Sockets:**
    A file descriptor that specifies an IP address and an associated port number, as well as the transfer protocol that will be used to handle the data.

- **Binding:**
    The process that takes place when an application or service uses an internet socket to handle the data it is inputting and outputting.

- **Listening:**
    A service is said to be "listening" on a port when it is binding to a port/protocol/IP address combination in order to wait for requests from clients of the service.
    Upon receiving a request, it then establishes a connection with the client (when appropriate) using the same port it has been listening on. Because the internet sockets used are associated with a specific client IP address, this does not prevent the server from listening for and serving requests to other clients simultaneously.

- **Port Scanning:**
    Port scanning is the process of attempting to connect to a number of sequential ports, for the purpose of acquiring information about which are open and what services and operating system are behind them. Common Ports Ports are specified by a number ranging from 1 to 65535.
    Many ports below 1024 are associated with services that Linux and Unix-like operating systems consider critical to essential network functions, so you must have root privileges to assign services to them. Ports between 1024 and 49151 are considered "registered". This means that they can be "reserved" (in a very loose sense of the word) for certain services by issuing a request to the IANA (Internet Assigned Numbers Authority). They are not strictly enforced, but they can give a clue as to the possible services running on a certain port. Ports between 49152 and 65535 cannot be registered and are suggested for private use. Because of the vast number of available ports, you won't ever have to be concerned with the majority of the services that tend to bind to specific ports.

## Common ports

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
tcpmux 1/tcp # TCP port service multiplexer echo 7/tcp echo 7/udp discard 9/tcp sink null discard 9/udp sink null systat 11/tcp users daytime 13/tcp daytime 13/udp netstat 15/tcp qotd 17/tcp quote msp 18/tcp # message send protocol . . . 
```

We will see in the section about nmap how to get a more complete list.

How To Check Your Own Open Ports There are a number of tools that can be used to scan for open ports.

One that is installed by default on most Linux distributions is netstat.

You can quickly discover which services you are running by issuing the command with the following parameters:

```bash
sudo netstat -plunt Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 785/sshd

tcp6 0 0 :::22 :::* LISTEN 785/sshd This shows the port and listening socket associated with the service and lists both UDP and TCP protocols.
```
