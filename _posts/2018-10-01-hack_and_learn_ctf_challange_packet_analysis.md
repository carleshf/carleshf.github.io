---
layout: post
published: false
title: Hack&Learn CTF Challange - Packet Analysis
---

In this post, I am going to explain how I answered the three _Packet Analysis_ challenges from the Capture the Flag proposed by [`Advoqt Academy`](https://www.advoqt.com). These challenges consisting in three blocks: the first set of four questions about basic understanding of [`Wireshark`](https://www.wireshark.org), and two blocks where we need to analyze two traces from [`Nmap`](https://nmap.org/).

```
├── Packet Analysis
│   ├── Wireshark 101
│   ├── NMAP Detector Part I
|   └── NMAP Detector Part II
```

This post is part of a series of 5. Here you have the links to each one:

1 __Hack&Learn CTF Challange - Packet Analysis__
2 [Hack&Learn CTF Challange - Web Hacking]()
3 [Hack&Learn CTF Challange - Forensic Investigation]()
4 [Hack&Learn CTF Challange - Password Cracking]()
5 [Hack&Learn CTF Challange - Cracking the network]()


## Wireshark 101

We are given a `.pcap` file. We have to answer some question from the content of the file.

#### A. How many packets are in the capture? 

We can see the total number of captured packages at the bottom right corner of `Wireshark`'s window. This first file contains `49961` packages.

![Wireshark - Total number of packages]({{baseurl}}/assets/wireshark101_01.png)

#### B. How many different 'Class A' IP addresses are in the packet capture? 

We first need to remember what are the Class A IP addresses. Class A IP addresses, where the 1st bit is 0, encompass the range of 0.0.0.0 to 127.255.255.255. This class is for large networks and has 8 bits for network and 24 bits for hosts.

Now, we can list all the IPs in the captured packages going to the menu `Statistics`, to the sub-menu `Endpoints` and then move to tab `IPv4`.

![Wireshark - IPs in captured session]({{baseurl}}/assets/wireshark101_02.png)

We can count the IPs in the Class A range, there are 15 IP addresses.

#### C. How many packets are using TCP Protocols?

We can filter the packages in the session by typing `tcp` in the filter box.

![Wireshark - Number of packages using TCP protocol]({{baseurl}}/assets/wireshark101_03.png)

Again, we can check the bottom right corner and see that there are `47021` packages using TCP protocols.

#### D. Which IP has sent or received the most number of packets?

At the `Endpoints` window, selecting the `IPv4` tab we can order the IPs by the number of packages.

![Wireshark - IPs per packages]({{baseurl}}/assets/wireshark101_04.png)

From the list, we can see that the IP `192.168.1.25` was the one who managed the largest number of packages.


## NMAP Detector Part I

The `.pcap` file for this challenge contains several `Nmap` scans. We have to find the IPs that are performing specific types of scanning. I found [this](http://www.hackingarticles.in/understanding-nmap-scan-wireshark/) article by _Raj Chande_ from [Hacking Articles](http://www.hackingarticles.in/) very illustrative for this challenge.

#### A.    What is the IP address doing a Stealth Scan with NMAP? 

The _Stealth Scan_ consist in sending `SYN` packages to a specific destination and port. If the port is open, the destination will respond with `SYN, ACK`. Then, it is required to send `RST` to tear down the connection.

If we filter the packages selecting those for TCP protocols we can then look for the `RST` packages.

![Wireshark - Stealth Scan]({{baseurl}}/assets/wireshark_nmap_p1_01.png)

Then, the IP `10.10.35.24` is the one doing a _Stealth Scan_. 

#### B.    What is the IP address doing a Null Scan with NMAP?

The _Null Scan_ sends TCP packages holding a sequence of "zeros" as flags. Since the destination will not know how to reply the request, the package will be discard indicating that port is open.

After filtering the packages for TCP protocols, we can look for the packages having `<None>` in the flags.

![Wireshark - Null Scan]({{baseurl}}/assets/wireshark_nmap_p1_02.png)

Then, the IP `10.10.20.10` is the one doing a _Null Scan_.

#### C.    What is the IP address doing an Xmas Scan with NMAP? 

Similar to the _Null Scan_, the _Xmas Scan_ manipulates the flags from the TCP header by setting up the `PSH`, `URG` and `FIN` flags. If the port of the receiver is open, then the package is discarded. If the port is closed, an `RST` is answered.

We filter the packages in the session by their flags, selecting those with the `xmas` flag (filter: `tcp.flags==0x029`).

![Wireshark - Xmas Scan]({{baseurl}}/assets/wireshark_nmap_p1_03.png)

This uncovers that the IP `10.10.50.61` is the one doing a _Xmas Scan_.

#### D.    What is the IP address doing a Fin Scan with NMAP?

A _Fin Scan_ is a light version of the _Xmas Scan_, where only the `FIN` flag is set. We filter the packages in the session selecting those with the `FIN` flag (filter: `tcp.flags==0x001`).

![Wireshark - Xmas Scan]({{baseurl}}/assets/wireshark_nmap_p1_04.png)

Then, the IP `10.10.20.34` is the one doing a _Fin Scan_.


## NMAP Detector Part II

The `.pcap` file for this challenge contains several `Nmap` scans. In this case, we need to apply different filters to discover the who is scanning who and how.

#### A.    What is the IP address sweeping the network 10.0.0.0/24?

If we apply a filter in the session's packages selecting as the destination all IPs in `10.0.0.0/24` (filter: `ip.dst==10.0.0.0/24`) we see that the IP `10.0.0.29` is sending different types of packages.

![Wireshark - Stealth Scan in 10.0.0.152]({{baseurl}}/assets/wireshark_nmap_p2_01.png)

#### B.    How many ports are open on IP 10.0.0.152 based on the scan?

If we filter the session packages by destination IP (filter: `ip.dst==10.0.0.152`) we see that the IP `10.0.0.152` has been targeted for a _Stealth Scan_.

![Wireshark - Stealth Scan in 10.0.0.152]({{baseurl}}/assets/wireshark_nmap_p2_02_01.png)

If we extend the filter to only show the `RST` responses we will have the list of the open ports that answered to the initial `SYN` package (filter: `ip.dst==10.0.0.152 and tcp.flags.reset==1`). 

![Wireshark - Stealth Scan in 10.0.0.152]({{baseurl}}/assets/wireshark_nmap_p2_02_02.png)

From the bottom right corner, we see that there are 61 packages, indicating that there are 61 open ports.

#### C.    What is the IP address executing an FTP bounce scan? 

A good description of how an FTP bounce scan works is [this](https://www.linux.org/threads/nmap-ftp-bounce-attack.4493/) post from _Jarret_ in [linux.org](https://www.linux.org).

Basically, an FTP server is used as an intermediary to query the destination server for open FTP ports. To uncover the `IP` of the FTP server, we filter the packages in the session by FTP protocol.

![Wireshark - FTP bounce scan]({{baseurl}}/assets/wireshark_nmap_p2_03.png)

This shows that `192.168.1.25` is the IP who started an FTP connection and it is using the `PORT` command to retrieve the open ports.

#### D.    Which IP address is doing a UDP port scan?

If we filter the packages in the session by UDP protocols we will see that the IP `192.168.1.20` is the one performing a _UDP port scan_.

![Wireshark - UDP port scan]({{baseurl}}/assets/wireshark_nmap_p2_04.png)

In _UDP port scan_, a destination will respond with a UDP packet, proving that it is open. If the port is closed, the destination will respond with an `ICMP` package (_port unreachable_).