---
layout: post
title:  "Scraping with Python - Connecting to the website you wish to scrape"
date:   2019-07-06 18:30:00 +1000
categories: programming
excerpt_separator: <!--more-->
---

Python provides us with two very powerful libraries to retrieve web pages from the web. There is the socket library to connect to remote webservers and retrieve data from such websites. There is also the urllib library which does this more succinctly. Let's take a deeper dive into these two libraries and how can we use to retrieve data from other websites.

<!--more-->

### What are sockets

Sockets are connections that we make over TCP/IP protocols. A good analogy to understand sockets is a phone-call. The process you follow to make a call is to dial another person and when they pick up the phone, connection is setup between you and the other person for the duration of that call. You can continue to speak and communicate with the other person till the call is operational and then once you finish off, you hang up, thereby closing the connection. 

Sockets are similar in the sense that you connect with another application(usually a web server) through a socket, continue to interact and communicate with that application and once you are done, you close the socket. 

### Setting up sockets in Python

```
import socket
mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
mysock.connect(('google.com',443)) # A function call with a tuple as a parameter
cmd = 'GET https://www.google.com/search?q=learning+python HTTP1.0\r\n\r\n.encode()'
mysock.send(cmd)
```

The encode() converts text in unicode to UTF-8 for transmission over the network. This is specifically required because Python stores all strings as unicode whereas the preferred character encoding on the internet and networks in general is the UTF-8. 

Once we have sent the request, we need to receive the response from the application

```
while True
	data_in_byte_array = mysock.recv(512)
	if len(data) < 1 :
		break
	response_in_unicode_str = data_in_byte_array.decode()
	print(response_in_unicode_str)
```

In fact when we receive the response, we generally receive it in the form of a byte-code. The `recv(512)` means we are to receieve 512 byte sized byte-arrays. What that means is that we receive is an array of bytes of length 512 and that the byte-array is most likely in UTF-8 format. The `decode()` function takes the coding (UTF-8 or UTF-2 etc) as the arguement, with UTF-8 being the default encoding. 

On decoding, it turns it into a normal string (unicode) which python can understand and work with. 

### Urllib 

Even though Python allows you to connect with websites over sockets, it also provides a level of abstraction through the Urllib library. Urllib allows us to connect and extract content from the website in a single command. 

```
import urllib.request, urllib.parse, urllib.error
fhand = urllib.request.urlopen('http://google.com/search?python')
for line in fhand:
	print(line.decode().strip())
```
