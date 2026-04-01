#Computer-network #communication-Principles 

#### 5.1.1 Communication Protocols

Communication in our daily lives takes many forms and occurs in many environments. We have different expectations depending on whether we are chatting via the internet or participating in a job interview. Each situation has its corresponding expected behaviors and styles.

Before beginning to communicate with each other, we establish rules or agreements to govern the conversation. These agreements include the following:

- What method of communication should we use?
- What language should we use?
- Do we need to confirm that our messages are received?

 **below are examples of determining the method, language, and confirmation strategies.**

---
#### Method

![[Pasted image 20260401102408.png]]

Before communication begins, we may have to select a method of communication to use

---
#### Language

![[Pasted image 20260401102448.png]]

And before a communication can begin, a agreed language for both is a must

---

#### Confirmation

![[Pasted image 20260401102524.png]]

Communication can only be considered successful when the intended message has reach and received by and confirmed by the destination

---

These rules, or protocols, must be followed in order for the message to be successfully delivered and understood. Among the protocols that govern successful human communication are these:

- ==An identified sender and receiver==
- ==Agreed upon method of communicating (face-to-face, telephone, letter, photograph)==
- ==Common language and grammar==
- ==Speed and timing of delivery==
- ==Confirmation or acknowledgment requirements==

The techniques that are used in network communications share these fundamentals with human conversations.

Think about the commonly accepted protocols for sending text messages to your friends.

---

#### 5.1.2 Why Protocols Matter

Just like us humans, computers use rules or protocols in order to communicate. Protocols are required for computers to properly understand the bits or data they send to each other across a network. 

In both wired and wireless network (or even both) all nodes within that network must "speak the same language" or to be precise, Protocols.

If everyone in the same room speak the same language each and everyone will have understanding and of-course if all speaks differently a network will collapse.

Networking protocols define many aspects of communication over the local network, ass shown in the table, these includes message format, message size, timing, encoding, encapsulation and message patterns.


|Protocol Characteristic|Description|
|---|---|
|**Message format**|When a message is sent, it must use a specific format or structure. Message formats depend on the type of message and the channel that is used to deliver the message.|
|**Message size**|The rules that govern the size of the pieces communicated across the network are very strict. They can also be different, depending on the channel used. When a long message is sent from one host to another over a network, it may be necessary to break the message into smaller pieces in order to ensure that the message can be delivered reliably.|
|**Timing**|Many network communication functions are dependent on timing. Timing determines the speed at which the bits are transmitted across the network. It also affects when an individual host can send data and the total amount of data that can be sent in any one transmission.|
|**Encoding**|Messages sent across the network are first converted into bits by the sending host. Each bit is encoded into a pattern of sounds, light waves, or electrical impulses depending on the network media over which the bits are transmitted. The destination host receives and decodes the signals in order to interpret the message.|
|**Encapsulation**|Each message transmitted on a network must include a header that contains addressing information that identifies the source and destination hosts, otherwise it cannot be delivered. Encapsulation is the process of adding this information to the pieces of data that make up the message. In addition to addressing, there may be other information in the header that ensures that the message is delivered to the correct application on the destination host.|
|**Message pattern**|Some messages require an acknowledgment before the next message can be sent. This type of request/response pattern is a common aspect of many networking protocols. However, there are other types of messages that may be simply streamed across the network, without concern as to whether they reach their destination.|
