---
layout:       single
title:        "Understanding Networks » Assignment 03: Packet Analysis"
date:         2019-10-28 19:47:23 -04:00
categories:   Understanding
classes:      wide
---

# Network

## Devices as seen by the router

To begin, I scraped device information as seen by the router. The first three devices listed (that is, the devices with the lowest IP addresses in the DHCP range: the router @ 192.168.1.10; the set top box @ 192.168.1.100; the wifi extender @ 192.168.1.153) are part of the base FiOS Residential service installation and the first devices to connect to the network when it was first configured.

```
Host:                            192.168.1.10
Connected To:                    Fios_Quantum_Gateway
Connection:                      Ethernet
IPv4 Address:                    192.168.1.10
Subnet Mask:                     255.255.255.0
Lease Type:                      Static
MAC Address:                     00:1b:a9:ac:11:26
Network Connection:              Bridge
Port Forwarding Services:        None

Host:                            IP-STB-1
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.100
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     00:23:a2:6e:24:d3
Network Connection:              Bridge
Port Forwarding Services:        tr69_002	tr69_003

Host:                            WECB-1025
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.153
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     18:1b:eb:95:7d:08
Network Connection:              Bridge
Port Forwarding Services:        tr69_001

Host:                            Kangaroo-Security-Sensor
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.246
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     30:ae:a4:e3:ed:14
Network Connection:              Bridge
Port Forwarding Services:        None

Host:                            My-iPhone5SE
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.151
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     4c:57:ca:7d:f8:93
Network Connection:              Bridge
Port Forwarding Services:        None

Host:                            Audreys-iPad
Connected To:                    Fios_Quantum_Gateway
Connection:                      Wireless 5G
IPv4 Address:                    192.168.1.5
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     6c:70:9f:67:8b:d0
Network Connection:              Bridge
Port Forwarding Services:        None
Connection Type:
Frequency:
Protocol Supported:
Radio Configuration (T x R : S):
PHY Rate / Modulation Rate:
RSSI:                            0
SNR:                             0

Host:                            apple-macbook-pro-13
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.244
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     88:e9:fe:77:ed:67
Network Connection:              Bridge
Port Forwarding Services:        None

Host:                            new-host-1
Connected To:                    Fios_Quantum_Gateway
Connection:                      Coax
IPv4 Address:                    192.168.1.242
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     a0:af:bd:57:c8:f3
Network Connection:              Bridge
Port Forwarding Services:        None

Host:                            Audreys-Air
Connected To:                    Fios_Quantum_Gateway
Connection:                      Wireless 5G
IPv4 Address:                    192.168.1.245
Subnet Mask:                     255.255.255.0
Lease Type:                      Dynamic
MAC Address:                     a4:83:e7:72:9c:1f
Network Connection:              Bridge
Port Forwarding Services:        None
Connection Type:
Frequency:
Protocol Supported:
Radio Configuration (T x R : S):
PHY Rate / Modulation Rate:
RSSI:                            0
SNR:                             0
```
**Observations / Questions**

* For some reason my ASUS C302C Chromebook was listed with the generic hostname 'new-host-1'. I haven't determined whether that was the router's fallback or because no hostname has been configured on the device. If the former, it may be a reflection of how aggressively Google locks down ChromeOS.
* Two out of nine devices (Audreys-iPad and Audreys-Air) have direct connections to the router's integrated wireless radio and therefore include fields for radio-specific information, unlike the other wireless devices connected to a wifi extender. Devices on the extender are designated as having a connection type 'coax'. The extender should really be operating as a 'dumb' repeater but I never managed to configure it properly.

## Devices as seen in the ARP cache on localhost

### After reboot, before connecting to wifi

```
$ arp -a
fios_quantum_gateway.fios-router.home (192.168.1.1) at c8:a7:a:a1:86:b9 on en0 ifscope [ethernet]
? (192.168.1.10) at 0:1b:a9:ac:11:26 on en0 ifscope [ethernet]
? (192.168.1.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
? (224.0.0.251) at 1:0:5e:0:0:fb on en0 ifscope permanent [ethernet]
```

**Observations / Questions**

* The ARP table on my localhost appears to have retained it's entry for the router after a reboot, but flushed its entries for other devices on the LAN. Why?

### After connecting to wifi

```
$ arp -a
fios_quantum_gateway.fios-router.home (192.168.1.1) at c8:a7:a:a1:86:b9 on en0 ifscope [ethernet]
audreys-ipad.fios-router.home (192.168.1.5) at 6c:70:9f:67:8b:d0 on en0 ifscope [ethernet]
? (192.168.1.10) at 0:1b:a9:ac:11:26 on en0 ifscope [ethernet]
? (192.168.1.14) at (incomplete) on en0 ifscope [ethernet]
? (192.168.1.22) at (incomplete) on en0 ifscope [ethernet]
my-iphone5se.fios-router.home (192.168.1.151) at 4c:57:ca:7d:f8:93 on en0 ifscope [ethernet]
? (192.168.1.214) at (incomplete) on en0 ifscope [ethernet]
audreys-air.fios-router.home (192.168.1.245) at a4:83:e7:72:9c:1f on en0 ifscope [ethernet]
? (192.168.1.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
? (224.0.0.251) at 1:0:5e:0:0:fb on en0 ifscope permanent [ethernet]
? (239.255.255.250) at 1:0:5e:7f:ff:fa on en0 ifscope permanent [ethernet]
```

**Observations / Questions**

* ARP entries are collected immediately after connecting to the network.

### After spending some time on the network

```
$ arp -a
fios_quantum_gateway.fios-router.home (192.168.1.1) at c8:a7:a:a1:86:b9 on en0 ifscope [ethernet]
audreys-ipad.fios-router.home (192.168.1.5) at 6c:70:9f:67:8b:d0 on en0 ifscope [ethernet]
? (192.168.1.120) at (incomplete) on en0 ifscope [ethernet]
my-iphone5se.fios-router.home (192.168.1.151) at 4c:57:ca:7d:f8:93 on en0 ifscope [ethernet]
? (192.168.1.167) at (incomplete) on en0 ifscope [ethernet]
apple-macbook-pro-13.fios-router.home (192.168.1.244) at 88:e9:fe:77:ed:67 on en0 ifscope permanent [ethernet]
audreys-air.fios-router.home (192.168.1.245) at a4:83:e7:72:9c:1f on en0 ifscope [ethernet]
kangaroo-security-sensor.fios-router.home (192.168.1.246) at 30:ae:a4:e3:ed:14 on en0 ifscope [ethernet]
? (192.168.1.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
? (224.0.0.251) at 1:0:5e:0:0:fb on en0 ifscope permanent [ethernet]
? (239.255.255.250) at 1:0:5e:7f:ff:fa on en0 ifscope permanent [ethernet]
```

## Devices seen by Herbivore

![Herbivore screenshot](/assets/images/undernets/2019-10-29/herbivore.png)

**Observations / Questions**

* Herbivore cannot 'see' devices on the LAN when it's running on the localhost and the local host is connected to a VPN. Why?
* Herbivore cannot see hostnames (device names) but it can lookup manufacturers based on the first 24 bits of the MAC addresses, aka the OUI (Organizational Unique Identifier).
* OUI lookup for the MacBook Air yielded no results. Why? Two possible explanations might be because
    a. it's a recently manufactured model and part of a recently assigned block of MAC addresses not yet registered with the lookup service used by Herbivore, or
    b. Herbivore is using a built-in table that hasn't been updated since Apple was assigned a new block

# How much of your network traffic is inbound vs. outbound?

## Packets over time

![Wireshark I/O Graph of packets over time](/assets/images/undernets/2019-10-29/io-graph_packets.png)

## Bytes over time

![Wireshark I/O Graph of bytes over time](/assets/images/undernets/2019-10-29/io-graph_bytes.png)

# What portion of it is HTTP traffic?

According to Wireshark's status bar, http packets accounted for a mere **0.1%** of packets captured (677 out of 662946) and TLS (encrypted http) accounted for **8.1%** (53824 out of 662946). At first glance that's a staggeringly small proportion although on further reflection, http/s requests and responses are usually just a function of webpages visited via browser.

![Wireshark I/O Graph of http packets vs all packets over time](/assets/images/undernets/2019-10-29/io-graph_http.png)

# How many devices are active on your network and what are their relative levels of activities?

There were six 'personal' devices on the network during capture, nine altogether if you count the television box (on standby), the wifi extender, and the router itself.

The number of packets originating from and/or destined for devices other than localhost (my laptop) were negligible in comparison.

Given Wireshark is running on my laptop, I would not have expected to capture packets unless they were either a. sent by or b. received by the localhost, in which case I'm going to assume the question is 'how does the volume of packets the localhost exchanged with other devices on the LAN compare on a device by device basis?'

## Packets by device vs all packets captured over time

![Wireshark I/O Graph of all packets by device over time](/assets/images/undernets/2019-10-29/io-graph_devices+outside.png)

## Packets by device captured over time

![Wireshark I/O Graph of packets exchanged by devices on the LAN](/assets/images/undernets/2019-10-29/io-graph_devices.png)

**Observations / Questions**

* I'm not sure what constitutes 'active' from a networking standpoint ... does that mean connected to the network? Awake as opposed to in some kind of suspended power mode)? Talking to other devices? Moving lots of packets?
* The largest volume of packets exchanged between the localhost and another device on the LAN was also an Apple product, a MacBook Air.

# GeoIP2 Map

I downloaded and installed the [MindMax GeoLite2 Free Downloadable Databases](https://dev.maxmind.com/geoip/geoip2/geolite2/) and imported them into Wireshark. It's a nice feature and the resultant map is actually generated as a webpage with some interactivity, including popup modal that lists IP addresses associated with each cluster.

![Map of GeoIP2 Data](/assets/images/undernets/2019-10-29/geoip2.png)

* Not unexpectedly, the cluster with the largest number of destinations showed up in Cheney Reservoir (Kansas), yet another indication how deficient IP location lookup can be. At least those provided by MindMax's free databases.

# Sequence of events during packet capture

* Power off devices
  * [ASUS C302C Chromebook](https://www.asus.com/us/2-in-1-PCs/ASUS-Chromebook-Flip-C302CA/)
  * iPhone5SE
* Laptop prep
  * stop wifi from auto-logon to home wifi
  * stop VPN from auto-logon
* Laptop reboot
  * open Wireshark
    * select interfaces
      * en0
      * utun1
      * lo0
    * click Capture
      * Input tab
      * Output tab
          * Capture to permanent file ...
          * Options tab
              * Resolve network names
              * Resolve transport names
        * start some apps
            * Evernote (3:21pm)
            * Chrome (3:22pm)
            * Herbivore
            * Update ASUS (4:28)
* Laptop
  * run `arp -a`
  * connect to wifi
  * run `arp -a`
  * turn on bluetooth (3:23)
* power on other devices
  * ASUS (3:24)
    * update Android apps
  * iPhone (3:25)
    * set timer for 90 minutes
    * run some apps
      * connect to [Bose SoundLink Color Bluetooth® speaker II](https://www.bose.com/en_us/products/speakers/portable_speakers/soundlink-color-bluetooth-speaker-ii.html) (3:46)
        * stream music from Bandcamp (3:51pm)
        * setup and configure [Kangaroo Motion Sensor](http://heykangaroo.com/products/motion-sensor) (3:38pm)
          * arm it (3:42pm)
          * trigger it (4:22pm, 4:57, 5:03, 5:09,5:14,5:24, 5:35)
* Laptop
  * connect USB Drive (~3:30pm)
  * download, install, and run avast (4:33)
  * run arp -a
  * run local jekyll
* after 90 minutes
 * connect laptop to VPN (5:02)
 * connect iPhone to VPN (5:02)
 * restart timer for 30 minutes

# To do

* What sites are the most common sources and destinations for your traffic?

# Gotchas and good to knows

* Wireshark's I/O Graph tool won't render a legend without a [workaround](https://ask.wireshark.org/question/3753/can-i-add-a-legend-to-an-io-graph/)
