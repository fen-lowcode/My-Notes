#Computer-network


**The Hypertext Transfer Protocol (HTTP)** is the [[Network Protocol]] programs use to communicate over the World Wide Web. There are many applications of HTTP, but HTTP is most famous for two-way conversation between web browsers and web servers.

---
#### **HTTP: The Internet’s Multimedia Courier**

Billions of JPEG images, HTML pages, text files, MPEG movies, WAV audio files, Java applets, and more cruise through the Internet each and every day. 

HTTP moves the bulk of this information quickly, conveniently, and reliably from web servers all around the world to web browsers on people’s desktops.

Because HTTP uses reliable data-transmission protocols, it guarantees that your data will not be damaged or scrambled in transit, even when it comes from the other side of the globe. 

This is good for you as a user, because you can access information without
worrying about its integrity. 

Reliable transmission is also good for you as an Internet application developer, because you don’t have to worry about HTTP communications being destroyed, duplicated, or distorted in transit. 

You can focus on programming the distinguishing details of your application, without worrying about the flaws and foibles of the Internet. 

Let’s look more closely at how HTTP transports the Web’s traffic.

Web content lives on web servers. Web servers speak the HTTP protocol, so they are often called HTTP servers. 

These HTTP servers store the Internet’s data and provide the data when it is requested by HTTP clients. The clients send HTTP requests to servers, and servers return the requested data in HTTP responses, as sketched in `Figure 1-1`.

Together, HTTP clients and HTTP servers make up the basic components of the World Wide Web.

![](http-client-server-model.png)


You probably use HTTP clients every day. The most common client is a web
browser, such as Microsoft Internet Explorer or Netscape Navigator. 

Web browsers request HTTP objects from servers and display the objects on your screen.

When you browse to a page, such as “http://www.oreilly.com/index.html,” your
browser sends an HTTP request to the server www.oreilly.com 
(see `Figure 1-1`). 

The server tries to find the desired object (in this case, “/index.html”) and, if successful, sends the object to the client in an HTTP response, along with the type of the object, the length of the object, and other information.

---

#### Resources

Web servers host web resources. A web resource is the source of web content. The simplest kind of web resource is a static file on the web server’s file-system. 

These files can contain anything: they might be text files, HTML files, Microsoft Word files, Adobe Acrobat files, JPEG image files, AVI movie files, or any other format you can think of.

However, resources don’t have to be static files. Resources can also be software programs that generate content on demand. These dynamic content resources can generate content based on your identity, on what information you’ve requested, or on the time of day. 

They can show you a live image from a camera, or let you trade stocks, search real estate databases, or buy gifts from online stores (see `Figure 1-2`).

![](http-resources-diagram.png)

In summary, a resource is any kind of content source. A file containing your company’s sales forecast spreadsheet is a resource. 

A web gateway to scan your local public library’s shelves is a resource. An Internet search engine is a resource.

---

#### Media Types

Because the Internet hosts many thousands of different data types, HTTP carefully tags each object being transported through the Web with a data format label called a MIME type. 

MIME (Multipurpose Internet Mail Extensions) was originally designed to solve problems encountered in moving messages between different electronic mail systems. 

MIME worked so well for email that HTTP adopted it to describe and label 1its own multimedia content.