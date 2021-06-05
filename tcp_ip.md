# THE TCP/IP PROTOCOL SUITE

The protocol stack used on the Internet is the Internet Protocol Suite. It is usually called TCP/IP after two of its most prominent protocols, but there are other protocols as well.


The TCP/IP model is based on a five-layer model for networking.
From bottom (the link) to top (the user application), these are the physical, data link, network, transport, and application layers.

## THE TCP/IP LAYERS

The TCP/IP protocol stack models a series of protocol layers for networks and systems that allows communications between any types of devices. The model consists of five separate but related layers, as shown in Figure below.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tcp_ip_5layers.PNG)


 The Internet protocol suite is based on these five layers. TCP/IP says most about the network and transport layers, and a lot about the application layer. TCP/IP also defines how to interface the network layer with the data link and physical layers, but is not directly concerned with these two layers themselves.


 Not all systems on a network need to implement all five layers of TCP/IP. Devices using the TCP/IP protocol stack fall into two general categories: a host or end system (ES) and an intermediate node (often a router) or an intermediate system (IS). The intermediate nodes usually only involve the first three layers of TCP/IP

 TCP/IP LAYERS IN BRIEF

 - Physical Layer: Contains all the functions needed to carry the bit stream over a physical medium to another system.

 - Data Link Layer: Organizes the bit stream into a data unit called a “frame” and delivers the frame to an adjacent system.  

 - Network Layer: Delivers data in the form of a packet from source to destination, across as many links as necessary, to non-adjacent systems.

 - Transport Layer: Concerned with process-to-process delivery of information.

 - Application Layer: Concerned with differences in internal representation, user interfaces, and anything else that the user requires


## ENCAPSULATION

The figure below showing how devices are only physically connected at the lowest layer (Layer 1). Note that functionally, intermediate nodes only require the bottom three layers of the model.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/protocols_interfaces.PNG)
