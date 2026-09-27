
### 1.8 Systems Communicate with Other Systems Using Networks


Up to this point in our tour of systems, we have treated a system as an isolated collection of hardware and software. In practice, modern systems are often linked to other systems by **[[Network Types | Networks]]**
  
From the point of view of an individual system, the network can be viewed as just another I/O device, as show to 

**When the system copies a sequence of bytes from main memory to the network adapter, the data flow across the network to another machine, instead of, say, to a local disk drive.** 

Similarly, the system can read data sent from other machines and copy these data to its main memory.

###### *[[Systems Communicate with Other Systems Using Networks|figure 1.14]]*

![[Pasted image 20260914060305.png]]


###### *[[Systems Communicate with Other Systems Using Networks|figure 1.15]]*

![[Pasted image 20260914060325.png]]



**==Using telnet to run hello remotely over a network==**

Returning to our hello example, we could use the familiar telnet application on a remote machine. 

Suppose we use a telnet client running on our local machine to connect to a telnet server on a remote machine. 

After we log in to the remote machine and run a shell, the remote shell is waiting to receive an input command. From this point, running the hello program remotely involves the five basic steps just like shown on *[[Systems Communicate with Other Systems Using Networks | Figure 1.15]]*.


* After we type in the hello string to the telnet client and hit the enter key, 

* The client sends the string to the telnet server. 

* After the telnet server receives the string from the network, it passes it along to the remote shell program. 

* Next, the remote shell runs the hello program and passes the output line back to the telnet server.

* Finally, the telnet server forwards the output string across the network to the telnet client, which prints the output string on our local terminal.

This type of exchange between clients and servers is typical of all network applications. In **Chapter 11** you will learn how to build network applications and apply this knowledge to build a simple Web server.
