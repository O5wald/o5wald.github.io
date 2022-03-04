---
title: "Client-Server Chat Room using Python"
draft: false
author: "Aryan Kapse"
---

# Client-Server Chat Room


In my [previous article](https://dr-deathvolt.github.io/2021-7-17-socket-programming-101), I covered what a socket is and the different types of sockets, as well as the C implementation. Today, we'll use Sockets to create a client-server chat room in Python.

Let's look at how client-server architecture works first.
The client-server architecture is a model in which the server hosts, provides, and controls the majority of the resources and services that the client consumes. One or more client computers are connected to a central server through a network or the internet in this architecture.

![client-server](https://www.quickhmi.com/files/slideshows/Network.png)


In our programme, we will create a basic server that will wait for a client to connect and then receive the messages from client and send it to other clients in the server. When someone disconnected then it broadcast the message that the particular client is disconnected.

Let's Dive into Coding!

## Server Side Coding

1. We need to import the necessary packages
  ```python
  # File : Server.py
  
  import socket 
  # to establish connection between server and client
  
  import threading 
  # to run multiple tasks at same time
  ```
2. Create socket object
```python
# server.py continue....

server = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
```
  - `socket.AF_INET` to define Address family
    - `AF_INET` for IPv4
    - `AF_INET6` for IPv6
  - `socket.SOCK_STREAM` to define type of socket
    - `SOCK_STREAM` for TCP/IP
    - `SOCK_DGRAM` for UDP
    - See all other types and their Information [here](https://www.ibm.com/docs/pl/aix/7.1?topic=protocols-socket-types)
3. Define Veriables for easy to use
```python
# server.py continue....

ADDRESS = '127.0.0.1'
PORT = 5550
```
  - `ADDRESS` variable defines the address that we want to host our server
    - we are building this for local area network `LAN`
    - so we are specifying local host address which is `127.0.0.1`
  - `PORT` to host on 5550 port
  - why not other ports?
    - 0 to 1024 ports are well known and predefined for services
    - You can see more info [here](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) on which port you want to use
    - you can use any port above 1024
4. Bind the server to address and port [means hosting the server on `ADDRESS` and `PORT`]
```python
# server.py continue....

server.bind((ADDRESS,PORT))
```
  - `bind()` assign an IP address and port to the server instance
    - `bind()` function needed tuple of address and port
5. Now, before start listening and accepting the clients. we need to define some functions to organize and simplify the code
```python
# server.py continue....

clients = []
names = []

def broadcast(messege):
  for client in clients:
    client.send(message.encode('utf-8')
```
  - creating list called `clients` and `names`
    - to store the IP addresses of connected clients
    - and there `names` 
  - defined a function called `broadcast()`
    - take `messege` as parameter
    - in every client in list of `clients`
    - send the messege in encoded form
    - Why? because sockets talk each other in bytes
6. Create function for revcieve and broadcast that revcived messege
```python
# server.py continue....

def recv_messege_send(client):
  while True:
    try:
       msg = client.recv(1024).decode('utf-8')
       broadcast(msg)
    except:
       index = clients.index(client)
       nm = names[index]
       print(f"{nm} left the server!")
       clients.remove(client)
       broadcast(f"{nm} left the server!")
       names.remove(nm)
       client.close()
       break
```
  - defined a function called `recv_send()`
    - which take `client` as an parameter then
    - try to check any messeage from the client and broadcast the messege
    - if any error occurs
      - error like client is left the server
      - client is down
    - then take index of that client and remove it from `clients` list and from `names` list
    - then broadcast the messeage that the particular client is left the server
    - `close()` for closing the connection of that client and break the loop so that server 
      - we won't recive any messege from that removed client anymore
7. We're good to go now for listen the clients
```python
# server.py continue....

server.listen(5)

while True: #0
    client,addr = server.accept() #1
    print(f"{addr} is Connected to the server!") #2
    broadcast(f"{addr} is Connected to the server!") #3
    client.send('NAME'.encode('utf-8')) #4
    nm = client.recv(1024).decode('utf-8') #5
    broadcast(f"Name of {addr} is {nm}") #6
    print(f"Name of {addr} is {nm}") #7
    clients.append(client) #8 
    names.append(nm) #9
    rec = threading.Thread(target=recv_messege_send,args=(client,)) #10
    rec.start() #11
```
  - `server.listen()` to listen the client is trying to connect
    - `listen()` function have parameter value 5
    - which means server listening upto 5 clients
    - You increase number of clients you want to connect to the server
  - Running a loop
    - at `#1` to accept the clients who wants to connect to the server
    - at `#1` in `client,addr` client store the information of the connected client and addr take only address of connected client
    - at `#2` and `#3` to simply just broadcast the messege that the client is connected
    - at `#4` asking the client to enter his nickname to display in server
    - at `#5` to `#9` reciving the name of that connected client decodeing it and storing in veriable then broadcasting the messege to tell the connected clients the name of the new connected client and then adding the new connected client address in `clients` list and name in `names` list
8. at `#10` we are using threading concept
  - threading means running multiple functions,tasks at same time
  - so in that `Thread` function we targeting the function `recv_messege_send` with `client` as an argument
  - so when the server waiting for the client to connect at that same time server is receving the messege from other clients which are already connected to the server and broadcast that messege
  - at `#11` we are starting that thread

## Client side coding

1. We need to import the necessary packages
  ```python
  # File : client.py
  
  import socket 
  # to establish connection between server and client
  
  import threading 
  # to run multiple tasks at same time
  ```
2. Take Name to display on the server
```python
# client.py continue....

name = input("Enter your name: ")
```
3. Defining Address and port which we want to connect
```python
# client.py continue....

ADDRESS = '127.0.0.1'
PORT = 5550
```
4. Create Socket Object
```python
# client.py continue....

client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
try: 
    client.connect((ADDR,PORT)) #1
except:
    print("Server is Curruntly Down! or Unreachable!") #2
    exit()
```
  - `socket.AF_INET` to define Address family
    - `AF_INET` for IPv4
    - `AF_INET6` for IPv6
  - `socket.SOCK_STREAM` to define type of socket
    - `SOCK_STREAM` for TCP/IP
    - `SOCK_DGRAM` for UDP
    - See all other types and their Information [here](https://www.ibm.com/docs/pl/aix/7.1?topic=protocols-socket-types)
  - **NOTE**: The address family and the type of socket must be same as server's address family and socket type
  - at `#1` we are trying to connect to the server
    - if any error occurs like server is not running or anything else
    - then printing the messege "Server is Curruntly Down! or Unreachable!" and exit the program
5. Creating simple function to send the messege to the server
  ```python
  # client.py continue....

  def send_msg(client):
    while True:
        try:
            msg = input()
            client.send(f"{name}: {msg}".encode('utf-8'))
        except:
            print("Something Went Wrong")
            exit()
```
- `send_msg` take client as an argument then run a while loop to take input from user and send that message with clients name and messege
- if any error occurs then print "Something Went Wrong" and exit the program

6. Creating simple loop to recive and able to run the `send_msg` function at same time
```python
# client.py continue....

while True:
    rec = client.recv(1024).decode('utf-8') #1
    if rec == 'NAME': #2
        client.send(name.encode('utf-8'))
    elif not rec: #3
        continue
    elif rec: #4
        print(f"{rec}")
    sm = threading.Thread(target=send_msg,args=(client,)) #5
    sm.start() #6
```
  - at `#1` client receiving the messege from server and storing it in the `rec`
  - at `#2` if messege received from the server contain 'NAME' which is asking for clients name then sending the `name`
  - at `#3` if messege received from the server doesn't contain any data then continue without printing it because server has no data to tell something
  - at `#4` any messege is received from the server then simply print it
  - at `#5` we are defining Thread Function to take `send_msg` function and run this function at same time as client is receving the messege
  - at `#6` simply starting that thread

---
# Testing
![client_server](/images/client_server.gif)
