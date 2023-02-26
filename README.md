# Server-Client
Simple client that sends files to a server 

## Description of the Project
#### In this project the student will write a client that sends the string 
#### “Hello, how are you today? I am fine thanks for asking” to my server. 
#### The client will send this message 2 bytes at a time.  
#### We will be using UDP to send the message.  
#### We will be exploring sending windows, sequence numbers, and ACKs in this program. 
#### The maximum number of bytes that can be in your ‘send window’ is 10 bytes. 
#### If you send more than the window size, the server complains. 
#### To make it interesting, if your client sends seq#0, and my server receives that correctly, my server will send a 0 to ack that it received the packet with seq number 0. 
#### NOTE this is different from normal TCP, but I wanted to vary it a bit. 
#### Occasionally, my server will drop a packet. Let’s say that you send packets with seq # 0, 2, 4. If I drop packet number 2, then when I receive packet with seq Number 4, I will drop it too. 
#### So, your client MUST timeout and resend packet 2, then 4, etc. I do not do any buffering on the server.  
#### The format (syntax) for the messages is as follows: 
#### From Client to server
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Message 1 from client to server: 4 bytes size of the message that is going to be sent (use htonl to encode)
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Message 2+ from client to server: a 17 byte string/buffer with:
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • the seq number in an 11 char field, don’t have to use htonl since it is a string 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • a 4 byte field with the size (no htonl) 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • followed by up to 2 bytes of the string (always send 2 bytes, except the last time if the string length is odd, you will send 1 byte) 
#### • Message from Client to server:
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • To make life easier, I do no ACK the size message. We will assume it arrives correctly!
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • All other messages:
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • If the correct seq number is received, the server will ACK that seq number (so if the client sends seq# 2, size 2, the server will ack 2) 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • This is sent in an 11 byte field (no htonl) as above 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Here is how I put the seq# and size  into the string
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • sprintf (bufferOut, "%11d%4d", (seqNumber), (sizeOfSendBuffer));
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • Here is how I pull them out on the other side
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; • sscanf(buffer, "%11d%4d", &seqNumber, &bytesSent);
#### The project MUST be written in “C” and must run on the university linux systems. 
#### The client and the server will make use of UDP DGRAM sockets.  
#### The server must be called server  and the client will be called client. 
#### The project must contain a makefile that allows for “make clean” as well as making the executables. 
#### The server will take 1 parameter, the portnumber it is listening on. 
#### The client will take 2 parameters, the ip address of the server, and the portnumber of the server 
#### The client should prompt the user for string to send. The server will exit once the string is received (one and done server) 
