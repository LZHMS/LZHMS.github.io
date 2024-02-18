---
title: Solution to OpenSSL Connection Problems With Github
date: 2023-10-23 23:27:15
toc: true
tags:
    - OpenSSL
categories: blog
excerpt: Solve OpenSSL connection problems with Github when using git tool to upload projects.
---
## Problems Uploading Files with Git

Sometimes we can use git tool to successfully upload projects to Github, but in other time especially after a period of configuration, we often meet the following error:
`OpenSSL SSL_read: Connection was reset, error 10054`
So this post is written to demonstrate this bug and give some solutions.

### What's the OpenSSL

Firstly, in the course of *Computer Network*, we had learned the HTTPS(Hypertext Transfer Protocol Secure) which works as authentication between client and server for data transmission. And the service of HTTPS works on SSL/TLS protocol.
SSL/TLS is a protocol for establishing a secure connection between a client and a server. It is used for authentication, encryption, and integrity check.

Therefore, OpenSSL is a software library for implementing the Secure Sockets Layer (SSL) and Transport Layer Security (TLS) protocols.

### The Relationship Between OpenSSL and IP Address

Many web servers and services use OpenSSL to secure communication over IP addresses. When you access a website via its IP address (e.g., an HTTPS website), OpenSSL may be used to encrypt and secure the data exchange.
SSL/TLS certificates, managed and validated by OpenSSL, are tied to specific domain names and IP addresses. When a client connects to a server using an IP address, the server's SSL/TLS certificate must match that IP address.

### What's Wrong with the Github

At the same time, Github uses OpenSSL to secure communication over IP addresses. But what the most import thing is that GitHub, like many other online services, may periodically change its IP addresses for various reasons, including security, network optimization, and load balancing.
In this case, if you use the previous IP address to access GitHub, you may encounter the above error.

## How to Solve the Problem

I. Check your Ip address of Github domain name in `hosts` file

```json
140.82.112.4 github.com
199.232.69.194 github.global.ssl.fastly.net
140.82.114.9 codeload.Github.com
```

We can utilize this website `https://www.ipaddress.com/ip-lookup` to check whether the IP address corresponds with its domain name.

II. Refresh DNS using PowerShell tools on your computer

```cmd
ipconfig /flushdns
```

III. Deactivate SSL authentication
If the above steps don' work, we have to deactivate SSL authentication which is not advised sometimes.

```bash
git config --global http.sslVerify "false"
```
