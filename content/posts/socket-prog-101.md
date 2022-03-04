---
title: "Socket Programming 101 using C"
draft: false
author: "Aryan Kapse"
---

# Socket Programming 101


# What is socket?

- Socket is one endpoint of a communication link between systems. Your application sends and receives all of its network data through a socket.

- There are few different socket **application programming interfaces (APIs)**.
  - **Berkeley Sockets**
    - released in 1983 with 4.3BSD Unix.
    - The terms Berkeley sockets, BSD sockets, Unix sockets, and Portable Operating System Interface (POSIX) sockets are often used interchangeably
    - If you're using Linux or macOS, then your operating system provides a proper implementation of Berkeley sockets.
  - **Winsock**
    - Windows socket API is called **Winsock**
    - it was created to be largely compatible with Berkeley sockets.


- **Historically, sockets were used for inter-process communication (IPC) as well as various network protocols. we use sockets only for communication with TCP and UDP**

# Types of Sockets

#### Sockets come in two basic types
- **Connection-oriented** and **connectionless**.

- Don't get confused with the term connectionless. Of course, two systems communicating over a network are in some sense connected. Keep in mind that these terms are used with special meanings, which we will cover shortly, and should not imply that some protocols manage to send data without a connection

- Protocols that are used Most
	- Transmission Control Protocol(**TCP**) (connection-oriented)
	- User Datagram Protocol(**UDP**)(connectionless)


#### Connectionless Protocol
- each data packet is addressed individually.
- From the protocol's perspective, each data packet is completely independent and unrelated to any packets coming before or after it
- like **UDP**
- UDP makes no guarantee that a packet will arrive. UDP doesn't generally provide a method to know if a packet did not arrive, and UDP does not guarantee that the packets will arrive in the same order they were sent

#### Connetion-Oriented Protocol
- It requires a logical connection to be established between the two processes before data is exchanged. The connection must be maintained during the entire time that communication is taking place, then released afterwards.
- The process is like telephone call, were virtual circuits is established, caller must know the person telephone number and phone must be answered before message can be delivered
- example TCP, 
- TCP needs established connections.

# Finally.... Socket Functions

- Common Socket Functions:
	- `socket()` creates and initialize a new socket.
	- `bind()` assigns local IP address and port number to the socket
	- `listen()` to listen for new connections [only for TCP]
	- `connect()` attempt to make a connection on a socket specific to the address
	- `accept()` create new socket for incoming TCP connection
	- `send()` and `recv()` used to send and receive data with socket
	- `sendto()` and `recvfrom()` used to send data from sockets without a bound remote address
	- `close()` (Berkeley sockets) and `closesocket()` (Winsock sockets) are used to close a socket
	- `shutdown()` used to close one side of TCP connection.
	- `select()` used to wait for an event on one or more sockets
	- `getnameinfo()` translates socket address to a node name an service location.
	- `getaddrinfo()` allocates and initializes a linked list of addrinfo structures for each network address that matches node an service.
	- `setsocketopt()` used to change some socket options

# Socket Setup

Before we can use sockets we need to include the socket API header files. these files vary depending on whether we are using Berkeley sockets or Winsock. 

**Note**: Winsock requires initialization before use, It also requires that a cleanup function is called when we are finished.

**We are writing code that compatible with both Linux/mac and Windows**

By using preprocessor statement 
```c
#if defined(_WIN32)
```
we can include in our program that will only compiled on Windows.

### Code for initializing socket

```c
/* file: socket_initi.c */

// When windows system detected
#if defined(_WIN32)
#ifndef _WIN32_WINNT
#define _WN32_WINNT 0x0600
# endif

//must defined for winsock headers to provide all function we need
#include<winsock2.h>
#include<ws2tcpip.h>
/*pragma statement tells the Microsoft Visual C compiler to link your program with Winsock Library ws2_32.lib*/
/* if you are using MinGW compiler then #pragma is ignored.*/
/* in this case you need to link manually using command line using -lws2_32*/
#pragma comment(lib, "ws2_32.lib")

#else
// when Linux/MacOS detected
//section includes Berkeley socket API headers and other headers needed for this platform
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<arpa/inet.h>
#include<netdb.h>
#include<unistd.h>
#include<errno.h>
#endif

#include<stdio.h>

int main(){
	
#if defined(_WIN32)
	WSADATA d;
	// WSAStartup() for Windows to initialize Winsock
	// MAKEWORD macro allow to request Winsock Version 2.2
	if (WSAStartup(MAKEWORD(2, 2), &d){			
		// If program unable to initialize Winsock
		fprintf(stderr, "Failed to initialize.\n");
		return 1;
		}
	
#endif
	// When all are done!
	printf("Ready to use socket API.\n");

#if defined(_WIN32)
	// Allows windows to do additional cleanup
	WSACleanup();
#endif
	return 0;
}
```

- Command for compiling and running this program on Linux/Mac
```console
gcc socket_initi.c -o socket_initi
```
- Compiling on Windows using MinGW
```console
gcc socket_initi.c -o socket_initi.exe -lws2_32
socket_initi.exe
```
Note : `-lws2_32` flag is needed to tell the compiler to link `ws2_32.lib`

