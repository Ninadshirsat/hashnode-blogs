---
title: "The 3-Way Handshake"
datePublished: Sun Feb 01 2026 15:03:21 GMT+0000 (Coordinated Universal Time)
cuid: cml3vex24000e02l7hldf60u6
slug: the-3-way-handshake
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769958085202/7782face-4939-4ce2-a33c-5b002082eaaa.png
tags: chaiaurcode, chaicode, chaicohort, tcp-handshake, 3wayhandshake

---

Imagine you want to send a very important document—perhaps a signed contract—to an office across the country.

If you just printed the 50 pages, threw them up into the air on a windy day, and hoped the wind would carry them to the destination, what would happen?

* Some pages would get lost forever.
    
* Some pages would arrive muddy and unreadable.
    
* Page 49 might arrive before page 2.
    
* The receiver wouldn't even know they were supposed to receive a contract, so they wouldn't know anything was missing.
    

This chaotic scenario is essentially what the internet looks like at its lowest level. The internet is a noisy, busy place where data "packets" frequently get dropped, corrupted, or arrive out of order.

If we want to browse the web, send emails, or watch Netflix without constant glitches, we need a system that imposes order on this chaos. We need a responsible adult to supervise the data transfer.

Enter **TCP**, the unsung hero of the internet.

### What is TCP and Why Do We Need It?

TCP stands for **Transmission Control Protocol**.

In simple terms, TCP is a set of rules (a protocol) that computers follow to ensure data gets from Point A to Point B reliably and correctly. It sits on top of the basic internet plumbing (IP, or Internet Protocol) to fix its shortcomings.

TCP is designed to solve four major problems of networked communication:

1. **Loss:** Data packets sometimes disappear in transit. TCP notices and resends them.
    
2. **Ordering:** Packets often take different routes and arrive scrambled. TCP puts them back in the correct 1, 2, 3 order.
    
3. **Corruption:** Sometimes data gets mangled along the way. TCP checks for errors and rejects bad data.
    
4. **Congestion:** If the network is too busy, TCP slows down so it doesn't make the traffic jam worse.
    

TCP is what we call a **connection-oriented** protocol. This means that before any real data is exchanged, the two computers must establish a formal connection. They have to agree to talk to each other.

How do they do that? Through a process called the "3-Way Handshake."

### The 3-Way Handshake: A Polite Conversation Analogy

Before computers send data, they need to verify that the other side is listening and ready.

Think of it like making an old-fashioned phone call to a friend, Bob. You don't just pick up the phone and immediately start reading a list of grocery items. You need to establish the connection first.

1. **You dial the number:** "Hello? Is this Bob? Are you there?"
    
2. **Bob picks up:** "Yes, this is Bob. I can hear you clearly. Can you hear me?"
    
3. **You confirm:** "Great, I hear you too. Okay, here is the grocery list..."
    

Only after step 3 does the actual information exchange begin.

In computer networking, this is exactly how TCP works. The client (your web browser, for example) wants to connect to a server (like Google's website computer).

Let’s walk through it technically, but slowly.

### Step-by-Step Working of SYN, SYN-ACK, and ACK

In TCP, instead of saying "Hello," computers send tiny packets with specific "flags" set. These flags act like indicators of the packet's intent.

* **SYN (Synchronize):** "Let's start a connection and synchronize our sequence numbers."
    
* **ACK (Acknowledge):** "I received your message."
    

Here is the actual flow of the 3-way handshake between your computer (Client) and a website (Server).

#### Step 1: The Connection Request (SYN)

Your computer wants to load a webpage. It sends a TCP packet to the server with the **SYN flag** turned on.

This packet also contains a random number, let's say **1000**. This is called a **Sequence Number**. It’s like saying: *"Hello Server, I want to connect. I will start numbering the data bytes I send you starting from number 1000."*

* **Analogy:** "Hello? Are you there?"
    

#### Step 2: The Acknowledgment and Counter-Offer (SYN-ACK)

The server receives your SYN packet. It sees that you want to connect. The server needs to do two things: acknowledge that it heard you, and open its own side of the connection.

It sends back a single packet with two flags turned on: **SYN** and **ACK**.

* **The ACK part:** The server sets an Acknowledgment Number to **1001**. (This means: "I received sequence number 1000. I am now waiting for byte number 1001.")
    
* **The SYN part:** The server also sends its *own* random Sequence Number, let's say **5000**. ("Also, I will start numbering the data bytes *I* send you starting from number 5000.")
    
* **Analogy:** "Yes, I hear you. I'm ready to talk too."
    

#### Step 3: The Final Confirmation (ACK)

Your computer receives the Server's SYN-ACK. It sees that the server is ready. Now, your computer needs to acknowledge that it heard the server's proposal.

Your computer sends one final packet containing just an **ACK flag**.

* It sets the Acknowledgment Number to **5001**. (This means: "I received your sequence number 5000. I am waiting for your byte number 5001.")
    
* **Analogy:** "Great, I hear you too. Let's begin."
    

Once this third packet is sent, the connection is **ESTABLISHED**. The handshake is complete, and real data can now flow freely.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769957770327/62fdbad1-05dc-47dd-9a42-22a27c2b4b31.png align="center")

### How Data Transfer Works: Ensuring Reliability, Order, and Correctness

Now that the connection is open, how does TCP ensure nothing goes wrong during the actual transfer?

The secret lies in those **Sequence Numbers** and **Acknowledgment (ACK) Numbers** we established during the handshake.

#### 1\. Keeping Order (Sequence Numbers)

Imagine you are downloading a large image. TCP breaks that image down into hundreds of small packets.

TCP assigns a **Sequence Number** to every byte of data sent. If the first packet contains 500 bytes of data, and it started at Sequence Number 1001, the data covers bytes 1001 through 1500.

If packets arrive out of order (e.g., packet #3 arrives before packet #2), the receiving computer looks at the sequence numbers and says, "Wait, I have a gap. I'll hold onto #3 until #2 arrives, then slot them together correctly."

* **Example:** It’s like numbering the pages of a book (Page 1 of 50, Page 2 of 50, etc.) before mailing them separately. The receiver can easily put the book back together in the right order.
    

#### 2\. Ensuring Delivery (Acknowledgments)

This is the core of TCP reliability. Every time a computer receives data, it *must* send an ACK back to the sender.

The ACK is cumulative. It tells the sender: **"I have received everything up to this point correctly. Please send me the next byte starting here."**

If the sender transmits 500 bytes of data, it sets a mental stopwatch. If it doesn't receive an ACK for those 500 bytes before the stopwatch runs out, it assumes the packet was lost.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769957898361/02276dc6-7a26-4734-afda-3526cb103ec7.jpeg align="center")

#### What Happens When Things Go Wrong? (Packet Loss and Retransmission)

Let's look at a real-world internet hiccup.

1. The Client sends Packet A (Sequence 1001).
    
2. The internet "eats" Packet A. It never reaches the Server.
    
3. The Server remains silent because it didn't receive anything to acknowledge.
    
4. The Client's stopwatch (retransmission timer) runs out.
    
5. **TCP Action:** The Client realizes something is wrong. It automatically **retransmits** Packet A (Sequence 1001).
    
6. This time, the Server gets it and sends back the ACK.
    

This mechanism ensures that even on a flaky connection, eventually, every piece of data arrives.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769957965783/98315887-6584-4116-ae2d-dc43d83c1603.jpeg align="center")

### Closing the Connection

Just as it's rude to hang up the phone mid-sentence, it's bad practice to just drop a TCP connection. When the data transfer is finished (e.g., the webpage has finished loading), the connection must be closed gracefully.

This is often called a four-step closure, using a flag called **FIN** (Finish).

1. **Client:** "I have no more data to send." (Sends FIN packet).
    
2. **Server:** "I acknowledge you are done." (Sends ACK packet). *(The server might still have a little bit of data left to send to the client, so it finishes that up).*
    
3. **Server:** "Okay, now I am also finished sending data." (Sends FIN packet).
    
4. **Client:** "I acknowledge you are finished. Goodbye." (Sends ACK packet).
    

The connection is now officially closed, freeing up resources on both computers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769958077144/44417d84-11af-4413-84db-b150cbc7c1d8.jpeg align="center")

### Summary

TCP is the rigid, bureaucratic, reliable backbone of the internet.

While other protocols exist that are faster but less reliable (like UDP, used for live video streaming where dropping a few frames doesn't matter), TCP is essential for things that *must* be correct: loading webpages, sending emails, transferring files, and banking.

It achieves this reliability not through magic, but through a very structured polite conversation:

1. **The 3-Way Handshake** to agree to talk (SYN, SYN-ACK, ACK).
    
2. Using **Sequence and ACK numbers** to track every byte sent and received.
    
3. Using **Timers** to detect lost packets and retransmit them.
    

Next time your webpage loads perfectly, spare a thought for the thousands of little TCP handshakes and acknowledgments happening behind the scenes to make it happen!