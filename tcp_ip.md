# THE TCP/IP PROTOCOL SUITE

The protocol stack used on the Internet is the Internet Protocol Suite. It is usually called TCP/IP after two of its most prominent protocols, but there are other protocols as well.


The TCP/IP model is based on a five-layer model for networking.
From bottom (the link) to top (the user application), these are the physical, data link, network, transport, and application layers.

## THE TCP/IP LAYERS

The TCP/IP protocol stack models a series of protocol layers for networks and systems that allows communications between any types of devices. The model consists of five separate but related layers, as shown in Figure below.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tcp_ip_5layers.PNG)


 The Internet protocol suite is based on these five layers. TCP/IP says most about the network and transport layers, and a lot about the application layer. TCP/IP also defines how to interface the network layer with the data link and physical layers, but is not directly concerned with these two layers themselves.




 TCP/IP LAYERS IN BRIEF

 - Physical Layer: Contains all the functions needed to carry the bit stream over a physical medium to another system.

 - Data Link Layer: Organizes the bit stream into a data unit called a “frame” and delivers the frame to an adjacent system.  

 - Network Layer or Internetwork Layer (IP): Delivers data in the form of a packet from source to destination, across as many links as necessary, to non-adjacent systems.

 - Transport Layer: Concerned with process-to-process delivery of information.

 - Application Layer: Concerned with differences in internal representation, user interfaces, and anything else that the user requires

Not all systems on a network need to implement all five layers of TCP/IP. Devices using the TCP/IP protocol stack fall into two general categories: a host or end system (ES) and an intermediate node (often a router) or an intermediate system (IS). The intermediate nodes usually only involve the first three layers of TCP/IP.


## ENCAPSULATION

The figure below showing how devices are only physically connected at the lowest layer (Layer 1). Note that functionally, intermediate nodes only require the bottom three layers of the model.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/protocols_interfaces.PNG)


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/encapsulation_headers.PNG)

    - The data link layer adds both a header and a trailer to the data it receives from the network layer.



## The Link layer

The Link layer needs to solve two basic problems when dealing with shared local area networks.


- How to send data using a shared medium, how to encode and send data across the link. E.g.,
    - If the link is wireless, engineers must agree on which radio frequencies are to be used to transmit data and how the digital data is to be encoded in the radio signal.
    - For wired connections, they must agree on what voltage to use on the wire and how fast to send the bits across the wire.
    - For Link layer technologies that use fiber optics, they must agree on the frequencies of light to be used and how fast to send the data.

- How to cooperate with other computers that might want to send data at the same time.    


# The Network Layer

Not all destination systems are directly reachable by the sender (directly
reachable systems are called **adjacent systems**, and adjacent systems are always “one hop away” from the sender; . A **hop** is the usual term used on the Internet or a router network to indicate the forwarding of a packet between one router and another or between a host and router)).

The biggest difference between the network layer and the data link layer is that the data link layer is in charge of data delivery between adjacent systems (directly connected systems one hop away), while the network layer delivers data to systems that are not directly connected to the source.

There can be many different types of data link and physical layers on the network, depending on the variety of the link types, but the network layer is essentially the same on all systems, end systems, and intermediate systems alike


## THE TRANSPORT LAYER


The transport layer breaks up a message at the sender into packets (each labeled “TL data” for transport-layer data and “TH” for transport-layer header) if necessary and reassemble the message at the receiver from the various segments that make up a message.


![](https://github.com/justinjiajia/img/blob/master/personalwiki/transport_layer_segments.PNG)


Process-to-process delivery is the task of the transport layer.

The network layer forwards each and every packet independently, and does not recognize any relationship between the packets. (Is this a file transfer or email packet? The network layer does not care.)

The transport layer, in contrast, can make sure the whole message, often strung out in a sequence of packets, arrives in order (packets can be delivered out of sequence) and intact (there are no errors in the entire message).

This function of the transport layer involves some method of flow control and error control (error detection and error correction) at the transport layer. The transport-layer protocol that performs all of these functions is TCP.

In many cases, the content of the packet forms a complete unit all by itself, called a datagram.  Self-contained datagrams are not concerned with sequencing or flow control, and these functions are absent in the User Datagram Protocol (UDP) at the transport layer.




## THE APPLICATION LAYER

the TCP/IP protocol stack really stops at the transport layer (where TCP and UDP are). It is up to the application programmer to decide what should happen at the client and server level at that point, although there are individual RFCs for guidance, such as for FTP

Although it is common to gather these TCP/IP applications into their own
layer, there really is no such thing in TCP/IP as an application layer to act as
some kind of “glue” between the application’s user and the network.


There are two major tasks that the application often needs to accomplish: **session support** and **conversion of internal representation**.


### SESSION SUPPORT

A session is a type of dialog controller between two processes that establishes, maintains, and synchronizes (controls) the interaction (dialog). A session decides if the communication can be half-duplex (both ends take turns sending) or fullduplex (both ends can send whenever they want). It also keeps a kind of “history” of the interaction between endpoints, so that when things go wrong or when the two communicate again, some information does not have to be resent. In practical terms, the session consists of all “state variables” necessary to construct the history of the connection between the two devices. It is more difficult, but not impossible, to implement sessions in a connectionless environment because there is no easy way to associate the variables with a convenient label.


### INTERNAL REPRESENTATION CONVERSION


The role of internal representation conversion is to make sure that the data exchange over the network is useful to the receivers. If the internal representation of data differs on the two systems (integer size, bit order in memory, etc.), the application layer translates between the formats so the application program does not have to. This layer can also provide encryption and compression functions, although it is more common to implement these last two functions separately from the network

## APPLICATIONS IN TCP/IP

Host TCP/IP implementations typically provide a range of applications that provide users with access to the data handled by the transport-layer protocols. These applications use a number of protocols that are not part of TCP/IP proper, but are used with TCP/IP. These protocols include the Hyper-Text Transfer Protocol (HTTP) used by Web browsers, the Simple Message Transfer Protocol (SMTP) used for email, the [Secure SHell (SSH)](ssh.md) for ... and many others.

Some protocols provide separate layers for sessions, internal representation conversion, and application services. In practice, these are seldom implemented independently.

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/protocols_layers.PNG)
