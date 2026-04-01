#Computer-network #Build-a-home-network 
#### 4.3.1 Wi-Fi Networks

A number of standards have been developed to ensure that wireless devices can communicate. They specify the RF spectrum used, data rates, how the information is transmitted, and more. The main organization responsible for the creation of wireless technical standards is the Institute of Electrical and Electronics Engineers (IEEE).

The **[[802.11]]** standard governs the WLAN environment. There are amendments to the IEEE 802.11 standard that describe characteristics for different standards of wireless communications. Wireless standards for LANs use the 2.4 GHz and 5 GHz frequency bands. Collectively these technologies are referred to as Wi-Fi.

Another organization, known as the Wi-Fi Alliance, is responsible for testing wireless LAN devices from different manufacturers. The Wi-Fi logo on a device means that this equipment meets standards and should operate with other devices that use the same standard.

Wireless standards are constantly improving the connectivity and speed of Wi-Fi networks. It is important to be aware of new standards as they are introduced because manufacturers of wireless devices will implement these standards quickly in their new products.

Do you have a wireless network in your home? Do you know what standards are supported by your wireless router?

---
#### 4.3.2 Wireless Settings

The Packet Tracer Basic Wireless Settings interface is shown in the figure. Wireless routers using the 802.11 standards have multiple settings that have to be configured. These settings include the following:

**Network Mode**

* Determines the type of technology that must be supported. For example,**802.11b**, **802.11g**,**802.11n** or **Mixed Mode.**

**Network Name (SSID)**

* Used to identify the WLAN. All devices that wish to participate in the WLAN must have the same SSID.

**Standard Channel**

* Specifies the channel over which communication will occur. By default, this is set to **Auto** to allow the access point (AP) to determine the optimum channel to use.

**SSID Broadcast**

* Determines if the SSID will be broadcast to all devices within range. By default, set to **Enabled**.

**Note:** SSID stands for Service Set Identifier.


![[Pasted image 20260401090533.png]]

---

**Network Mode**

The 802.11 protocol can provide increased throughput based on the wireless network environment. If all wireless devices connect with the same 802.11 standard, maximum speeds can be obtained for that standard. If the access point is configured to accept only one 802.11 standard, devices that do not use that standard cannot connect to the access point.

A mixed mode wireless network environment can include devices that use any of the existing Wi-Fi standards. This environment provides easy access for older devices that need a wireless connection but do not support the latest standards.

When building a wireless network, it is important that the wireless components connect to the appropriate WLAN. This is done using the SSID.

The SSID is a case-sensitive, alphanumeric string that contains up to 32 characters. It is sent in the header of all frames transmitted over the WLAN. The SSID is used to tell wireless devices, called wireless stations (STAs), which WLAN they belong to and with which other devices they can communicate.

We use the SSID to identify a specific wireless network. It is essentially the name of the network. Wireless routers usually broadcast their configured SSIDs by default. The SSID broadcast allows other devices and wireless clients to automatically discover the name of the wireless network. When the SSID broadcast is disabled, you must manually enter the SSID on wireless devices.

Disabling SSID broadcasting can make it more difficult for legitimate clients to find the wireless network. However, simply turning off the SSID broadcast is not sufficient to prevent unauthorized clients from connecting to the wireless network. All wireless networks should use the strongest available encryption to restrict unauthorized access.

---

#### 4.4.1 First Time Setup

Many wireless routers designed for home use have an automatic setup utility that can be used to configure the basic settings on the router. These utilities usually require a PC or laptop to be connected to a wired port on the router. If no device is available that has a wired connection, it may be necessary to configure the wireless client software on the laptop or tablet first.

To connect to the router using a wired connection, plug an Ethernet patch cable into the network port on the computer. Plug the other end into a LAN port on the router. Do not plug the cable into the port or interface that is labeled “Internet”. The internet port will connect to the DSL or cable modem. Some home routers may have a built-in modem for internet connections. If this is the case, verify that the type of connection is correct for your internet service. A cable modem connection will have a coaxial terminal to accept a BNC-type connector. A DSL connection will have a port for a telephone-type cable, usually an RJ-11 connector.

After confirming that the computer is connected to the network router and the link lights on the NIC indicate a working connection, the computer needs an IP address. Most network routers are set up so that the computer receives an IP address automatically from a local DHCP server automatically configured on the wireless router. If the computer does not have an IP address, check the router documentation and configure the PC or tablet with a unique IP address, subnet mask, default gateway, and DNS information.

---

#### 4.4.2 Design Considerations

Before entering the configuration utility, or manually configuring the router through a web browser, you should consider how your network will be used. You do not want to configure the router and have that configuration limit what you are able to do on the network, nor do you want to leave your network unprotected.


**What should my network e called**

* If SSID broadcasting is on, the SSID name will be seen by all wireless clients within your signal range. Many times the SSID gives away too much information about the network to unknown client devices. It is not a good practice to include the device model or brand name as part of the SSID. Wireless devices have default settings that are easy to find on the internet, as well as known security weaknesses.

**What types of devices will attach to my network**

* Wireless devices contain radio transmitter/receivers that function within a specific frequency range. If a device only has the necessary radio for 802.11 b/g, it will not connect if the wireless router or access point is configured to only accept 802.11n or 802.11ac standards. If all devices support the same standard, the network will work at its optimum speed. If you have devices that do not support the n or ac standards, then you will have to enable legacy mode. A legacy mode wireless network environment varies between router models but can include a combination of 802.11a, 802.11b, 802.11g, 802.11n, and 802.11ac. This environment provides easy access for legacy devices that need a wireless connection.

**How do i add new devices**

* The decision regarding who can access your home network should be determined by how you plan to use the network. On some wireless routers, it is possible to set up guest access. This is a special SSID coverage area that allows open access but restricts that access to using the internet only.

The figure shows a wireless setup screen.

**Note:** Some wireless routers may label legacy mode as mixed mode.

![[Pasted image 20260401092634.png]]

---
#### 4.4.3 Video - Wireless Router and Client Configuration

![[Cisco Networking Academy Course Launch(6).mp4]]

