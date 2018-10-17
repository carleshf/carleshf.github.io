---
layout: post
published: false
title: Hack&Learn CTF Challange - Forensic Investigation
---

In this post, I am going to explain how I answered the four _Forensic Investigation_ challenges from the Capture the Flag proposed by [`Advoqt Academy`](https://www.advoqt.com). These challenges consisting of four blocks that follow the history of the spy _Ann Dercover_. After being released, Ann Dercover disappears. But the investigators were carefully monitoring her network activity and now we have the duty to find her.

```
├── Forensic Investigation
│   ├── Ann's Rendezvous Case
│   ├── Ann's Rendezvous Case II
│   ├── Ann's Rendezvous Case III
|   └── Ann's Rendezvous Case IV
```

This post is part of a series of 5. Here you have the links to each one:

1 [Hack&Learn CTF Challange - Packet Analysis]()
2 [Hack&Learn CTF Challange - Web Hacking]()
3 __Hack&Learn CTF Challange - Forensic Investigation__
4 [Hack&Learn CTF Challange - Password Cracking]()
5 [Hack&Learn CTF Challange - Cracking the network]()


__Considerations__

* Internal Network: `192.168.30.0/24`
* DMZ: `10.30.30.0/24`
* The "Internet": `172.30.1.0/24`


## Ann's Rendezvous Case

Evidence: Investigators have found that Ann's laptop has the MAC address `00:21:70:4D:4F:AE`.

#### A. Find the IP address of her laptop

We filter the captures packages to see the DCHP packages selecting her MAC addess (filter: `bootp and eth.addr==00:21:70:4D:4F:AE`, DHCP uses [BOOTP](https://wiki.wireshark.org/BOOTP) as its transport protocol).

![Ann's Case 1 - Ann's IP]({{baseurl}}/assets/ann_dercover_p1_01.png)

From the result, we see that her IP is `192.168.30.108`.

#### B. What is the name of her laptop?

We filter the captured packages selecting the NetBIOS Name Service (NBNS) to see the content of the packages(filter: `nbns and ip.addr==192.168.30.108`).

![Ann's Case 1 - Laptop's name]({{baseurl}}/assets/ann_dercover_p1_02.png)

From the result, we see that the name of her laptop is `ANN-LAPTOP`.

#### C. What is the MAC address of the router that gives her access to the internet?

From the DCHP packages from _Question A_, we can see an `ACK` package from `192.168.30.10`.

#### D. What is the IP of her DNS?

We filtered the traffic for DNS (filter: `dns`). We observed that the `10.30.30.20` is acting as DNS and it is located in the DMZ.

![Ann's Case 1 - DNS' IP]({{baseurl}}/assets/ann_dercover_p1_04.png)

#### E. How many devices were in the network including the router?

We can see all the IPs in her network by going to the menu `Statistics`, to the sub-menu `Endpoints` and then move to tab `IPv4`.

![Ann's Case 1 - IP in her network]({{baseurl}}/assets/ann_dercover_p1_05.png)

Here we see that there are 7 IPs. But the IP `192.168.30.255` is a broadcast IP, so there are 6 devices in her network.

#### F. What is the IP address of one of the email servers?

We filtered the traffic for IMAP.

![Ann's Case 1 - IP in her network]({{baseurl}}/assets/ann_dercover_p1_06.png)

From the results, we see that the e-mail server is `205.188.58.10``.

## Ann's Rendezvous Case II

## Ann's Rendezvous Case III

## Ann's Rendezvous Case IV