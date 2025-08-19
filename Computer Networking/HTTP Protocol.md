#Computer-network


**The Hypertext Transfer Protocol (HTTP)** is the [[Network Protocol]] programs use to communicate over the World Wide Web. There are many applications of HTTP, but HTTP is most famous for two-way conversation between web browsers and web servers.

---

Billions of JPEG images, HTML pages, text files, MPEG movies, WAV audio files,
Java applets, and more cruise through the Internet each and every day. 

HTTP moves the bulk of this information quickly, conveniently, and reliably from web servers all around the world to web browsers on people’s desktops.

Because HTTP uses reliable data-transmission protocols, it guarantees that your data will not be damaged or scrambled in transit, even when it comes from the other side of the globe. 

This is good for you as a user, because you can access information without
worrying about its integrity. 

Reliable transmission is also good for you as an Internet application developer, because you don’t have to worry about HTTP communications being destroyed, duplicated, or distorted in transit. 

You can focus on programming the distinguishing details of your application, without worrying about the flaws and foibles of the Internet. 

Let’s look more closely at how HTTP transports the Web’s traffic.

Web content lives on web servers. Web servers speak the HTTP protocol, so they are often called HTTP servers. 

![](http-client-server-model.png)

These HTTP servers store the Internet’s data and provide the data when it is requested by HTTP clients. The clients send HTTP requests to servers, and servers return the requested data in HTTP responses, as sketched in `Figure 1-1`.

Together, HTTP clients and HTTP servers make up the basic components of the World Wide Web.