---
layout: post
title:  "Playing with Maximum TCP/IP Port #"
date:   2019-04-02 14:00:40 -0700
categories: Networking
---

The collection of communication protocols used on the internet is referred to as TCP/IP. TCP stands for transmission control protocol and IP is obviously Internet Protocol. Sure, I know the definition ... but, what about practical knowledge? That's why I've used a Raspberry Pi to create a simple TCP/IP server.

First, I'll need to SSH into my Pi.

```console
dave@Ryzen3:~$ ssh pi@192.168.1.3
pi@192.168.1.3's password:
Linux raspberrypi 4.14.98-v7+ #1200 SMP Tue Feb 12 20:27:48 GMT 2019 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Apr  4 15:38:45 2019 from 192.168.1.2
```

I found a [cool TCP/IP demo](https://www.monocilindro.com/2017/03/22/how-to-run-a-c-tcpip-server-on-raspberry-pi/) that I'll be replicating on my own. It involves
  * running C code on the Pi to create a TCP/IP server
  * using PuTTY to connect to the TCP/IP server

Before I continue, I want to explain that I *do not know how to code in C* ... all I've done here is make port # changes to an established tutorial. First, I'll be creating the folder and file for this demo:

```console
pi@raspberrypi:~ $ mkdir TCP_Demo
pi@raspberrypi:~ $ cd TCP_Demo/
pi@raspberrypi:~/TCP_Demo $ nano TCP_Server_Demo.c
```

This is the C code from the tutorial that I put in the file:

```c
#include<stdio.h>
#include<sys/socket.h>
#include<arpa/inet.h> //inet_addr

int main()
{
    int socket_desc , new_socket , c;
    struct sockaddr_in server , client;

    //Create socket
    socket_desc = socket(AF_INET , SOCK_STREAM , 0);
    if (socket_desc == -1)
    {
        printf("Could not create socket");
    }

    //Prepare the sockaddr_in structure
    server.sin_family = AF_INET;
    server.sin_addr.s_addr = INADDR_ANY;
    server.sin_port = htons( 8888 );

    //Bind
    if( bind(socket_desc,(struct sockaddr *)&server , sizeof(server)) < 0)
    {
        puts("bind failed");
    }
    puts("bind done");

    //Listen
    listen(socket_desc , 3);

    //Accept and incoming connection
    puts("Waiting for incoming connections...");
    c = sizeof(struct sockaddr_in);
    new_socket = accept(socket_desc, (struct sockaddr *)&client, (socklen_t*)&c);
    if (new_socket<0)
    {
        perror("accept failed");
    }

    puts("Connection accepted");

    return 0;
}
```

Here's how I made that code an executable file:

```console
pi@raspberrypi:~/TCP_Demo $ gcc TCP_Server_Demo.c -o Executable_TCP_Server_Demo.c
./Executable_TCP_Server_Demo.c
```

This is how I ran that executable:

```console
Executable_TCP_Server_Demo.c  TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ ./Executable_TCP_Server_Demo.c
bind done
Waiting for incoming connections...
```

^It's waiting for a connection, so I'll connect to it with PuTTY from Windows. Note that the port # is 8888 and recall that the IP address of my Pi is 192.168.1.3. Here's the PuTTY connection that'll properly connect:

![port-8888-connection](/assets/2019-04-04-TCP-IP-Raspberry/port-8888-connection.PNG)

 This is the pi output pre and post the TCP/IP connection:

 ![connection-accepted](/assets/2019-04-04-TCP-IP-Raspberry/connection-accepted.PNG)

The connection was accepted, the Pi posted the "Connection accepted" message, and the TCP communication gets closed.

This got me thinking ... what's the max port # that can be used? Apparently it's 65,535 because that's (2^16)-1 ... we'd expect 65,535 to be allowable, but what about 65,536? What happens when I try out 65536 instead of the initial 8888? I changed the initial "server.sin_port = htons( 8888 );" line to:

```c
server.sin_port = htons( 65536 );
```

^When I try compiling this "impossible" program I get an error titled "warning: large integer implicitly truncated to unsigned type".

```console
pi@raspberrypi:~/TCP_Demo $ cp TCP_Server_Demo.c Impossible_TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ nano Impossible_TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ ls
Executable_TCP_Server_Demo.c  Impossible_TCP_Server_Demo.c  TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ gcc Impossible_TCP_Server_Demo.c -o Executable_Impossible_TCP_Server_Demo.c
Impossible_TCP_Server_Demo.c: In function ‘main’:
Impossible_TCP_Server_Demo.c:20:30: warning: large integer implicitly truncated to unsigned type [-Woverflow]
     server.sin_port = htons( 65536 );
                              ^~~~~
pi@raspberrypi:~/TCP_Demo $ ls
Executable_Impossible_TCP_Server_Demo.c  Impossible_TCP_Server_Demo.c
Executable_TCP_Server_Demo.c             TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ ./Executable_Impossible_TCP_Server_Demo.c
bind done
Waiting for incoming connections...
```

The program appears to have compiled despite the error, BUT I can't seem to connect to it using port # 65536.

![cannot-open-address](/assets/2019-04-04-TCP-IP-Raspberry/cannot-open-address.PNG)

I'm still able to PuTTY connect to ip address 192.168.1.3 on port 22 (not pictured), so I haven't broken everything ... I did some reading and apparently this error comes up when a type can't handle a number because the number is outside of the allowable range ... I'm wondering ... is 65535 allowed?

```c
server.sin_port = htons( 65535 );
```

That compiled! ... can I connect to it from Windows in PuTTY?!? I CAN! IT WORKS!!!!!! :D

```console
pi@raspberrypi:~/TCP_Demo $ cp Impossible_TCP_Server_Demo.c Maybe_TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ nano Maybe_TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ gcc Maybe_TCP_Server_Demo.c -o Executable_Maybe_TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ ls
Executable_Impossible_TCP_Server_Demo.c  Impossible_TCP_Server_Demo.c
Executable_Maybe_TCP_Server_Demo.c       Maybe_TCP_Server_Demo.c
Executable_TCP_Server_Demo.c             TCP_Server_Demo.c
pi@raspberrypi:~/TCP_Demo $ ./Executable_Maybe_TCP_Server_Demo.c
bind done
Waiting for incoming connections...
Connection accepted
pi@raspberrypi:~/TCP_Demo $
```

I mostly replicated a previously published tutorial on creating a simple TCP/IP server with a Raspberry Pi, BUT I threw in a cool new twist ... what's the max port number that I can connect to? Apparently it's 65,535! 65,536 won't compile, but 65,535 works just fine! :D
