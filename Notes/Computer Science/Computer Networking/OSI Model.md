#Computer-network 

`Source:` https://www.youtube.com/watch?v=Ilk7UXzV_Qc


The Open System Interconnection, the model have 7 layers which are Application, Presentation, Session, Transport, Network, Data Link and Physical.

These layers are provided by a mixture of drivers, Operating systems, Application and networking hardware that facilitates the transmission of data through data transfer medium like fiber optics, WiFi etc. 

#### **Layer 7**

**The Application layer** is the layer where it provides services for the end users these services are protocols that work with data the client are using, one of these protocols may be HTTP which is used with most of the browsers and web, and software that use this layer, these application offers services that allow the application layer to supply data to and receive data from The Presentation layer.

#### **Layer 6**

**The Presentation layer**, this layer performs the uncomplicated task of syntax processing (converting one data format to another). 

For example, you ordered something from an online store, these transaction is typically handled in a secure transmission which means the data passing between the store or the website application will transmit encrypted data to the presentation layer that will be decrypted and processed.

This layer handles translating the data from the top layer, which is presented in application format, network format, vice versa. after processing the information is then passed to Session layer or Application Layer depending whenever the data is being transmitted or receiving.

#### **Layer 5**

**The Session layer,** The construction, direction and conclusion of connection between devices occur.
This layer supports multiple types connection as well as being responsible for authentication and re-connection. after the connection has been established the data is transferred to or from the Transport layer.

#### **Layer 4**

**The Transport layer**, is responsible for the transmission of data across network connections. This layer coordinates how much data to send, how fast, where it goes and these sort of things and all, one of the most widely used protocols used on this layer is the **Transmission Control Protocol** or **User Datagram Protocol**, other protocol may provide other capabilities like error recovery, re-transmission etc.


#### **Layer 3**

**The Network layer** handles the routing of the data, After the data arrives at this layer each frames of data is examined to conclude if the data has reached it's final location, the layer sends data to the correct destination on outgoing transmission and receives incoming transmission as well. The IP portion of **TCP/IP** is the commonly known network layer for the internet this layer also manages the mapping between logical addresses and physical addresses. For IP addresses this is accomplished through Address Resolution protocol or **ARP**. The data is then passed to the next required layer which is the data link layer.


#### **Layer 2**

**The Data link layer**, is the most complex. This layer is divided into sub layers called the Media Access Control or MAC and Logical Link Control or LLC. 

The layer sets up links across the physical network when this layer receives data from the physical layer it checks for transmission errors and then packages the bits into data frames, from there this layer manages the physical addressing methods for the MAC or LLC layers An example of the MAC layer includes 802.11 wireless specification also Ethernet. at the data link layer it then passes data from or to the final layer which is the Physical layer.

#### **Layer 1**

**The Application layer**, this is the electrical or the physical layer of the model, This layer encompasses the network cables, power plugs, cable pin out, radio frequencies, pulses of light, repeaters, routers etc.

When troubleshooting this is where usually the first place to start.


___

The OSI model is the guide for developers and inventors to smooth the progress of developing communication products and software programs that will work in cooperation a commonly established model.

Once you understand the model you can then understand which protocol and devices will be compatible with one another.