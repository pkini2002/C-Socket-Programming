## Socket Programming 

Socket Programming is a way of connecting 2 nodes on a network to communicate with each other. One socket (ie; the Server) listens on a particular port at an IP, while the other socket (ie; the Client) reaches out to the other to form a connection. 

Socket programming is widely used in instant messaging applications, binary streaming, document collaborations & online streaming platforms.

If we are creating a connection between the client & server using TCP then it has few functionalities like: TCP is suited for applications that require high reliability. It is used by protocols like HTTP, HTTPS, FTP, SMTP & Telnet. TCP rearranges the data packets in the order specified. Therefore, there is a absolute guarantee that the data transferred remains intact in the way it was sent. 

TCP does flow control & requires 3 packets to setup a socket communication before any user data can be sent. TCP handles reliability & congestion control. It also does error checking & error recovery. 

| Image | Description |
|-------|-------------|
| <img width="330" height="375" alt="image" src="https://github.com/user-attachments/assets/59891f65-4930-4036-925d-7089d3f94577" /> | **TCP Server**: Using `create()`, Creates a TCP Socket. Using `bind()`, binds the socket to server address. Using `listen()` it waits for the client to approach the server to make a connection. Using `accept()` the connection is established between client & server and it is ready to transfer the data. <br><br> **TCP Client**: It creates TCP Socket & connects to the server. |

## Necessity of the header file imported

| Header File | Description |
|-------|-------------|
| stdio.h | Used for standard input/output operations. Used for `printf()` to display messages in the console or to use `getchar()` to read server's response from the keyboard. |
| netdb.h | Used for network database operations. Commonly used for DNS lookups & Network Services. |
| netinet/in.h | Purpose is for Internet address family structures. <br><br> - `struct sockaddr_in`: IPv4 socket address structure <br> - `INADDR_ANY`: Binds to all available interfaces | 
| stdlib.h | Used for common library functions like `exit()` to terminate programs with status code |
|string.h | Used for string manipulation functions like <br><br> - `bzero()`: Clear/zero out buffer memory <br> - `strncmp()`: Compare strings |
| sys/socket.h | This library contains core socket programming interfaces like `socket()`, `bind()`, `accept()`, `listen()`, Socket constants like `AF_INET` & `SOCK_STREAM`|
| sys/types.h | Used for system datatypes like `socketlen_t` |
| unistd.h | UNIX standard functions like `read()`, `write()`, `close()`|
| arpa/inet.h | Used for Internet Operations & address conversions. `inet_addr` converts the IP address string to binary network format. |


## Main Components of TCP Server Program

- Creates a TCP Socket `SOCK_STREAM` and with IPV4 addressing `AF_INET`
- Configures server address & listens on all available network interfaces (`INADDR_ANY`) and at port 8080
 ```
 servaddr.sin_family = AF_INET;
 servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
 servaddr.sin_port = htons(PORT);

```
- Binds the socket to IP address & port
```
bind(sockfd, (SA*)&servaddr, sizeof(servaddr))
```
- Server starts listening for connections. Queue upto 5 pending connections.
```
listen(sockfd, 5)
```
- Accepts all incoming client connections & blocks until a client connects. Returns a new socket descriptor for communication
```
connfd = accept(sockfd, (SA*)&cli, &len);
```
- Reads message from the client into the buffer
```
read(connfd, buff, sizeof(buff));
```
- Display's the client message
```
printf("From client: %s\t To client : ", buff);
```
- Reads server response from keyboard
```
while ((buff[n++] = getchar()) != '\n');
```
- Sends server response back to the client
```
write(connfd, buff, sizeof(buff));
```
- Exits the chat loop if the server types `exit`
```
if (strncmp("exit", buff, 4) == 0)
```
- Closes the socket after chat ends
```
close(sockfd);
```

## Main Components of TCP Client Program

- Socket creation & Server address Configuration: Creates a TCP socket with IPv4 addressing with PORT number set to 8080 <br><br>
inet_addr() converts string IP to network byte order
htons() converts port number to network byte order
```
sockfd = socket(AF_INET, SOCK_STREAM, 0);
servaddr.sin_family = AF_INET;
servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
servaddr.sin_port = htons(PORT);
```

- Connect to the server (Client uses connect() instead of bind(), listen(), accept())
Initiates a TCP connection to the server & blocks until the connection is established or fails.
```
connect(sockfd, (SA*)&servaddr, sizeof(servaddr))
```

## Output




