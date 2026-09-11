#Computer-network #network-protocol

From HTTP The Definitive Guide Book

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

Web servers attach a MIME type to all HTTP object data (see Figure 1-3). 

When a web browser gets an object back from a server, it looks at the associated MIME type to see if it knows how to handle the object. 

Most browsers can handle hundreds of popular object types: 

* Displaying image files
* Parsing and formatting HTML files
* Playing audio files through the computer’s speakers
* Launching external plug-in software to handle special formats.

---

A MIME type is a textual label, represented as a primary object type and a specific
subtype, separated by a slash. For example:

• An HTML-formatted text document would be labeled with type text/html.
• A plain ASCII text document would be labeled with type text/plain.
• A JPEG version of an image would be image/jpeg.
• A GIF-format image would be image/gif.
• An Apple QuickTime movie would be video/quicktime.
• A Microsoft PowerPoint presentation would be application/vnd.ms-powerpoint.

There are hundreds of popular MIME types, and many more experimental or limited-
use types.

==`A very thorough MIME type list is provided in Appendix D at book: HTTP The defeenitive Guide.pdf`==

---
#### URIs

Each web server resource has a name, so clients can point out what resources they
are interested in. The server resource name is called a uniform resource identifier, or URI. URIs are like the postal addresses of the Internet, uniquely identifying and locating information resources around the world. 

Here’s a URI for an image resource on Joe’s Hardware store’s web server:

example: http://www.joes-hardware.com/specials/saw-blade.gif

Figure 1-4 shows how the URI specifies the HTTP protocol to access the saw-blade
GIF resource on Joe’s store’s server. 

Given the URI, HTTP can retrieve the object. URIs come in two flavors, called URLs and URNs. Let’s take a peek at each of these types of resource identifiers now.

#### URLs 

The uniform resource locator (URL) is the most common form of resource identifier.
URLs describe the specific location of a resource on a particular server. 

They tell you exactly how to fetch a resource from a precise, fixed location. 

Figure 1-4 shows how a URL tells precisely where a resource is located and how to access it. Table 1-1 shows a few examples of URLs.

![[Pasted image 20260321110834.png]]

| URL | Description |
|-----|-------------|
| http://www.oreilly.com/index.html | The home URL for O’Reilly & Associates, Inc. |
| http://www.yahoo.com/images/logo.gif | The URL for the Yahoo! website’s logo |
| http://www.joes-hardware.com/inventory-check.cgi?item=12731 | The URL for a program that checks if inventory item #12731 is in stock |
| ftp://joe:tools4u@ftp.joes-hardware.com/locking-pliers.gif | The URL for the locking-pliers.gif image file, using password-protected FTP as the access protocol |

Most URLs follow a standardized format of three main parts:
• The first part of the URL is called the scheme, and it describes the protocol\
  used to access the resource. This is usually the HTTP protocol (http://).

• The second part gives the server Internet address (e.g., www.joes-hardware.com).

• The rest names a resource on the web server (e.g., /specials/saw-blade.gif ).
  Today, almost every URI is a URL.

---

#### Transactions

Let’s look in more detail how clients use HTTP to transact with web servers and
their resources. An HTTP transaction consists of a request command 

(sent from client to server), and a response result (sent from the server back to the client). 

This communication happens with formatted blocks of data called HTTP messages, as
illustrated in Figure 1-5.

![[Pasted image 20260321112402.png]]


---
#### Methods

HTTP supports several different request commands, called HTTP methods. Every
HTTP request message has a method. The method tells the server what action to per-
form (fetch a web page, run a gateway program, delete a file, etc.).

Table 1-2 lists five common HTTP methods.

| HTTP Method | Description                                                     |
| ----------- | --------------------------------------------------------------- |
| GET         | Send named resource from the server to the client.              |
| PUT         | Store data from client into a named server resource.            |
| DELETE      | Delete the named resource from a server.                        |
| POST        | Send client data into a server gateway application.             |
| HEAD        | Send the HTTP headers from the response for the named resource. |

---
#### Status Code

Every HTTP response message comes back with a status code. The status code is a
three-digit numeric code that tells the client if the request succeeded, or if other actions are required. A few common status codes are shown in Table 1-3.

| HTTP Status Code | Description                                      |     |
| ---------------- | ------------------------------------------------ | --- |
| 200              | OK. Document returned correctly.                 |     |
| 302              | Redirect. Go someplace else to get the resource. |     |
| 404              | Not Found. Can’t find this resource.             |     |

HTTP also sends an explanatory textual “reason phrase” with each numeric status
code (see the response message in Figure 1-5). The textual phrase is included only for descriptive purposes; the numeric code is used for all processing.

The following status codes and reason phrases are treated identically by HTTP soft-
ware:

```
200 OK
200 Document attached
200 Success
200 All’s cool, dude

```

HTTP status codes are explained in detail in Chapter 3.

---
#### Web Pages Can Consist of Multiple Objects\

An application often issues multiple HTTP transactions to accomplish a task. 

For example, a web browser issues a cascade of HTTP transactions to fetch and display a graphics-rich web page. The browser performs one transaction to fetch the HTML “skeleton” that describes the page layout, then issues additional HTTP transactions for each embedded image, graphics pane, Java applet, etc. 

These embedded resources might even reside on different servers, as shown in Figure 1-6. Thus, a “web page” often is a collection of resources, not a single resource.

![[Pasted image 20260321113855.png|550]]

---

#### HTTP Messages 

Now let’s take a quick look at the structure of HTTP request and response mes-
sages. We’ll study HTTP messages in exquisite detail in Chapter 3.

HTTP messages are simple, line-oriented sequences of characters. Because they are
plain text, not binary, they are easy for humans to read and write.

`Figure 1-7 shows the HTTP messages for a simple transaction.`

![[Pasted image 20260321113955.png]]

HTTP messages sent from web clients to web servers are called request messages.
Messages from servers to clients are called response messages. 

There are no other kinds of HTTP messages. The formats of HTTP request and response messages are very similar.

==HTTP messages consist of three parts:==

**Start line:**

* The first line of the message is the start line, indicating what to do for a request or what happened for a response.

**Header fields:**

* Zero or more header fields follow the start line. Each header field consists of a name and a value, separated by a colon (:) for easy parsing. The headers end with a blank line. Adding a header field is as easy as adding another line.

**Body:**

* After the blank line is an optional message body containing any kind of data.
  Request bodies carry data to the web server; response bodies carry data back to
  the client. Unlike the start lines and headers, which are textual and structured, the body can contain arbitrary binary data (e.g., images, videos, audio tracks, software applications). Of course, the body can also contain text.

---

#### Simple Message Example

`Figure 1-8` shows the HTTP messages that might be sent as part of a simple transaction. The browser requests the resource http://www.joes-hardware.com/tools.html.

`In Figure 1-8`, the browser sends an HTTP request message. The request has a GET
method in the start line, and the local resource is /tools.html. 

The request indicates it is speaking Version 1.0 of the HTTP protocol. The request message has no body, because no request data is needed to GET a simple document from a server.

The server sends back an HTTP response message. The response contains the HTTP
version number (HTTP/1.0), a success status code (200), a descriptive reason phrase
(OK), and a block of response header fields, all followed by the response body 
containing the requested document. 

The response body length is noted in the Content-Length header, and the document’s MIME type is noted in the Content-Type header.

![[Screenshot_20260321_114609.png]]

### Connections

Now that we’ve sketched what HTTP’s messages look like, let’s talk for a moment
about how messages move from place to place, across Transmission Control Protocol
(TCP) connections.

---
### TCP/IP

HTTP is an application layer protocol. HTTP doesn’t worry about the nitty-gritty
details of network communication; instead, it leaves the details of networking to
TCP/IP, the popular reliable Internet transport protocol.

TCP provides:
• Error-free data transportation
• In-order delivery (data will always arrive in the order in which it was sent)
• Unsegmented data stream (can dribble out data in any size at any time)

The Internet itself is based on TCP/IP, a popular layered set of packet-switched network protocols spoken by computers and network devices around the world. TCP/IP
hides the peculiarities and foibles of individual networks and hardware, letting computers and networks of any type talk together reliably.

Once a TCP connection is established, messages exchanged between the client and
server computers will never be lost, damaged, or received out of order.
In networking terms, the HTTP protocol is layered over TCP. HTTP uses TCP to
transport its message data. Likewise, TCP is layered over IP `(see Figure 1-9).`

![[Pasted image 20260321114914.png]]


### Connections, IP Addresses, and Port Numbers

