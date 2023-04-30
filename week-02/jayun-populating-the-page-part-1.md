# Populating the page: how browsers work (Part 1)

## Table of Contents

- [1. Introduction](#1-introduction)
  - [1-1. To provide better user experiences](#1-1-to-provide-better-user-experiences)
- [2. Overview](#2-overview)
  - [2-1. Latency](#2-1-latency)
    - [Network latency](#network-latency)
    - [Developers' goals](#developers--goals)
  - [2-2. Browsers are single-threaded](#2-2-browsers-are-single-threaded)
    - [single-threaded](#single-threaded)
    - [Developers' goals](#developers--goals-1)
    - [Render time is key](#render-time-is-key)
    - [Web performance can be improved by doing these](#web-performance-can-be-improved-by-doing-these)
- [3. Navigation](#3-navigation)
  - [3-1. Navigation Step 1. DNS Lookup](#3-1-navigation-step-1-dns-lookup)
    - [Process of DNS Lookup](#process-of-dns-lookup)
    - [Once per hostname](#once-per-hostname)
  - [3-2. Navigation Step 2. TCP Handshake](#3-2-navigation-step-2-tcp-handshake)
    - [TCP: Transmission Control Protocol](#tcp--transmission-control-protocol)
    - [Three-way handshaking](#three-way-handshaking)
  - [3-3. Navigation Step 3. TLS Negotiation](#3-3-navigation-step-3-tls-negotiation)
- [4. Response](#4-response)
  - [4-1. TTFB (Time to First Byte)](#4-1-ttfb--time-to-first-byte-)
  - [4-2. TCP Slow Start / 14KB rule](#4-2-tcp-slow-start---14kb-rule)
  - [4-3. Congestion control](#4-3-congestion-control)
- [5. Upcoming presentation - Part 2](#5-upcoming-presentation---part-2)

## 1. Introduction

> As web developers, we need to know how browsers work. But why specifically do we need to know, and how can it help us?

### 1-1. To provide better user experiences

- Fast sites provide better user experiences.
- Users want and expect web experiences with content that is fast to load and smooth to interact with.
- Understanding how browsers work can help us optimize our code to improve website performance - **faster load times and smoother user experiences**!

<br />

## 2. Overview

> Before we dive into how the browser works, let's talk about two important issues in web performance.

### 2-1. Latency

#### Network latency

- Network latency is the time it takes to transmit bytes over the air to computers.
- Latency is the biggest threat to our ability to ensure a fast-loading page.

#### Developers' goals

- To make the site load as fast as possible
- At least appear to load super fast
- So the user gets the requested information as quickly as possible.

### 2-2. Browsers are single-threaded

#### single-threaded

- Browsers execute a task from beginning to end before taking up another task.

#### Developers' goals

- For smooth interactions, ensure performance site interactions, from smooth scrolling to being responsive to touch.

#### Render time is key

- Ensuring the main thread can complete all the work we throw at it
- And still always be available to handle user interactions

#### Web performance can be improved by doing these

- By understanding the single-threaded nature of the browser
- By minimizing the main thread's responsibilities, where possible and appropriate
- By ensuring rendering is smooth and responses to interactions are immediate

<br />

## 3. Navigation

> Navigation is the first step in loading a web page.

- It occurs whenever a user requests a page by entering a URL into the address bar, clicking a link, submitting a form, as well as other actions.
- One of the goals of web performance is **to minimize the amount of time a navigation takes to complete**.
  - In ideal conditions, this usually doesn't take too long, but latency and bandwidth are foes that can cause delays.

### 3-1. Navigation Step 1. DNS Lookup

> The first step of navigating to a web page is **finding where the assets for that page are located**.

#### Process of DNS Lookup

- If you navigate to `https://example.com`, the HTML page is located on the server with IP address of `93.184.216.34`.
  - If you've never visited this site, a DNS lookup must happen.
- Browser requests a DNS lookup, which is eventually fielded by a **name server**, which in turn responds with an **IP address**.
  - After this initial request, **the IP will likely be cached for a time**, which speeds up subsequent requests by retrieving the IP address from the cache instead of contacting a name server again.

#### Once per hostname

- DNS lookups usually only need to be done once per hostname for a page load.
- DNS lookups must be done for each unique hostname the requested page references.

  - If your fonts, images, scripts, ads, and metrics all have different hostnames, **a DNS lookup will have to be made for each one**.
    → This can be problematic for performance, particularly on mobile networks.

  ![image](https://user-images.githubusercontent.com/85419343/232291254-7189bd3c-012a-4bd9-9c29-841cf510d615.png)

  - When a user is on a mobile network, each DNS lookup has to go **from the phone to the cell tower to reach an authoritative DNS server**.
    - The distance between a phone, a cell tower, and the name server can add significant latency.

### 3-2. Navigation Step 2. TCP Handshake

> Once the IP address is known, the browser sets up a connection to the server via a **TCP three-way handshake**.

#### TCP: Transmission Control Protocol

- To control the transmission of the data in a reliable way
- Two entities attempting to communicate - in this case the browser and web server - can **negotiate the parameters of the network TCP socket connection before transmitting data**, often over HTTPS.

#### Three-way handshaking

- SYN, SYN-ACK, ACK
  - Three messages transmitted by TCP to negotiate and start a TCP session between two computers

### 3-3. Navigation Step 3. TLS Negotiation

> Navigation is not yet finished…

- For secure connections established over HTTPS, another “handshake” is required.
- This handshake, or rather the TLS negotiation, determines which cipher will be used to encrypt the communication, verifies the server, and establishes that a secure connection is in place before beginning the actual transfer of data.
  - This requires three more round trips to the server **before the request for content is actually sent.😩**

![image](https://user-images.githubusercontent.com/85419343/232291398-8da73a63-ad62-41cb-a7ff-abe57bb6baa8.png)

- While making the connection secure adds time to the page load, **a secure connection is worth the latency expense**.🔐
  - Because the data transmitted between the browser and the web server **cannot be decrypted by a third party**.😎

> 💡 **After the 8 round trips**, the browser is finally(🥲) able to make the request.

<br/>

## 4. Response

> Once we have an **established connection to a web server**, the browser sends an initial HTTP GET request on behalf of the user, which for websites is most often an HTML file.
> Once the server receives the request, it will reply with **relevant response headers and the contents of the HTML.**

### 4-1. TTFB (Time to First Byte)

- Response for the initial request contains **the first byte of data received**.
- TTFB is the time between **when the user made the request** - say by clicking on a link - and **the receipt of this first packet of HTML**.
- The first chuck of content is usually 14KB of data.

### 4-2. TCP Slow Start / 14KB rule

- The first response packet will be 14KB.
  - This is part of **TCP slow start**, an algorithm that balances the speed of a network connection.
- Slow start **gradually increases the amount of data transmitted** until the network's maximum bandwidth can be determined.
- In [TCP slow start](https://developer.mozilla.org/en-US/docs/Glossary/TCP_slow_start), after receipt of the initial packet, the server **doubles** the size of the next packet to around 28KB.
  - Subsequent packets increase in size until a predetermined threshold is reached, or congestion is experienced.

![image](https://user-images.githubusercontent.com/85419343/232291565-2a2a60bb-8ac9-44ad-9a14-f3ddcc037178.png)

- TCP slow start gradually builds up transmission speeds appropriate for the network's capabilities to avoid congestion.👍

### 4-3. Congestion control

- As the server sends data in TCP packets, the user's client confirms delivery by returning acknowledgments, or ACKs.
- The connection has a limited capacity depending on hardware and network conditions.
- If the server sends too many packets too quickly, they will be dropped.
  - Meaning, there will be no acknowledgment.
  - The server registers this as missing ACKs.
- Congestion control algorithms use this flow of sent packets and ACKs **to determine a send rate**.

<br />

## 5. Upcoming presentation - Part 2

- An upcoming presentation next week will go into more detail on the browser rendering process.
  - Parsing, DOM, CSSOM, Render Tree, Layout, Paint and metrics used to measure performance.

---

### Reference

- https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work
