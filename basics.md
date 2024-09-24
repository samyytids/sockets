## What is a socket

A socket is just a way of communicating with anothe program. In Unix this is done by reading or writing to a file descriptor. This is basically the way that everything is done in Unix, since basically anything can be thought of as a file descriptor. 

### How do I actually use this information?
Well the long and short of it is that to get the file descriptor we need we make a call to the socket() system routing. Which returns the socket descriptor and we communicate through it using the send() and recv() socket calls. 

These special socket calls are basically the equivalent to the standard read and write calls for other file descriptors. But these let you have more control over the data that you send. 

## What is an internet socket?

Well there are 2 types of internet socket (at least as far as I am conserned). I should look up "Raw sockets" once I am done. 

### Stream sockets
"SOCK_STREAM"
These are for two way connect communication streams, if I send (1, 2) the other will receive (1, 2) in that order (apparently this is mostly done error free). 
telnet and ssh both use these as do any HTTP clients. 

#### How do they work?
They use TCP (the thing I am actually interested in)! 
https://datatracker.ietf.org/doc/html/rfc793 <--- for way too much info on TCP. 
The long and short is that TCP ensures that data arrives sequentially and error-free. 

### Datagram sockets
"SOCK_DGRAM"
These are often called connectionless sockets, but why?
Datagrams make no guarantees about if something will be received or what order it will be received, but if you do receive something it will be error free. 
Datagrams do not use TCP they use UDP.
Because I just accidentally undid all of this I shall paraphrase:
- They don't need a continuous connection.
- You package up data, fire it to a destination and hope.
- There are no guarantees all your packets will arrive or that they will arrive in the right order. 
- The only guarantee is what will arrive will be error free. 

These are why Peter gets packet loss in Apex. They are used in things like online games. 

Some of the issues with data vanishing can be avoided by using ACK packets that basically say, hey sender I got your packet send me the next one. If no ACK is received the sender will simply send the original packet again. 

The main reason for using UDP is speed. Here we are trading potential packet loss for tolerable ping.Think about it when playing a game if I am sending my location 175 times a second, do I really care if 3 of them go missing?

## How does networking work at a low level?
We start by lowering protocols.
[Ethernet[IP[UDP[TFTP[DATA]]]]]]
We'll start with something very important...

### Data encapsulation
When a packet is created it is wrapped in a header, sometimes but rarely also a footer. This is done by the first protocol. This is then wrapped by the next and the next and the next until we reach the hardware layer. 
All these headers are then stripped upon being received. This is all key to...

### Layered Network Model ISO/OSI

The main advantage mentioned here is that this system allows for the socket programmer to not care how a message vame to them since all the layers before them will have stripped away their headers and left them only with the headers they need to interact with at their level.

Here are the layers:
- Application <--- My app, Least physical
- Presentation
- Session
- Transport
- Network
- Data link
- Physical <--- for example my ethernet cable, Most physical

Above is very general. In Unix specifically:
- Application (telnet, ftp, etc)
- Host-to-Host transport layer (UDP, TCP)
- Internet Layer (IP and routing)
- Network access layer (ethernet, wifi or whatever)

Thankfully most of these layers are handled by things I never intend to touch...



















