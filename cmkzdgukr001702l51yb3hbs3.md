---
title: "DNS Resolution Explained Step by Step Using dig"
datePublished: Thu Jan 29 2026 11:29:54 GMT+0000 (Coordinated Universal Time)
cuid: cmkzdgukr001702l51yb3hbs3
slug: dns-resolution-explained-step-by-step-using-dig
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769613207254/e92bfba6-31be-4e58-b8b9-2ccc7bd7e09a.jpeg
tags: dns, dig, dns-resolution, chaicode, chaicohort, dns-commands

---

## What is DNS and DNS Resolution

* When we type a name like [google.com](http://google.com) or [https://ninad-shirsat-portfolio.vercel.app/](https://ninad-shirsat-portfolio.vercel.app/) into our browser, we magically land on that particular webpage. But the real magician behind this magic is something called DNS— Domain Name System.
    
* To put it simply, DNS is like our phone’s contact app. We tap on “Dad,” and our phone knows it should dial `+42-43422322`. DNS does the same thing we type `google.com` , and it finds the server IP address so our browser can connect to it.
    
* DNS resolution is the process of associating names and IP addresses, and it's one of the most essential services on a network.
    
* People understand descriptive names, but network communications require difficult-to-remember addresses. While it's simple enough for network administrators to connect to *webserver3*, a computer needs the IP address of the destination server to establish communication.
    

  
To understand how DNS resolution works, we’ll first break down the different parts of the DNS system: recursive resolver, root servers, TLD servers, and authoritative servers, and then use practical examples `dig` commands to see how each part works in real time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769683589292/1a9dcb78-99b8-48cc-8b24-1585d0c4458b.png align="center")

## Recursive Resolver

**Recursive resolver,** also know as **DNS Resolver** is the DNS server that does the hard work of finding an answer on behalf of the client. When a browser asks for a domain name, the resolver takes full responsibility for resolving it by querying root, TLD, and authoritative name servers until it gets the final IP address. Once resolved, it caches the result for a period of time to make future requests faster.

## Root name servers

The authoritative name servers that serve the DNS root zone, commonly known as the “root servers”, are a network of hundreds of servers in many countries around the world.

Operators who manage a DNS recursive resolver typically need to configure a *“root hints file”*. This file contains the names and IP addresses of the root servers,

The administration of the Domain Name System (DNS) is structured in a hierarchy using different managed areas or “zones”, with the root zone at the very top of that hierarchy. Root servers are DNS nameservers that operate in the root zone. These servers can directly answer queries for records stored or cached within the root zone, and they can also refer other requests to the appropriate Top Level Domain (TLD) server. The TLD servers are the DNS server group one step below the root servers in the DNS hierarchy, and they are an integral part of resolving DNS queries.

![DNS Heirarchy](https://www.cloudflare.com/img/learning/dns/glossary/dns-root-server/dns-root-server.png align="left")

A common misconception is that there are only 13 root servers in the world. In reality, there are many more, but still only 13 IP addresses used to query the different root server networks. Limitations in the original architecture of DNS require there to be a maximum of 13 server addresses in the root zone. In the early days of the Internet, there was only one server for each of the 13 IP addresses, most of which were located in the United States.

Today, each of the 13 IP addresses has several servers, which use [Anycast routing](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) to distribute requests based on load and proximity. Right no,w there are over 600 different DNS root servers distributed across every populated continent on earth.

## TLD name servers

A TLD nameserver maintains information for all the domain names that share a common domain extension, such as .com, .net, or whatever comes after the last dot in a URL. For example, a .com TLD nameserver contains information for every website that ends in ‘.com’. If a user was searching for [google.com](http://google.com), after receiving a response from a root nameserver, the recursive resolver would then send a query to a .com TLD nameserver, which would respond by pointing to the authoritative nameserver for that domain.

## Authoritative name servers

When a recursive resolver receives a response from a TLD nameserver, that response will direct the resolver to an authoritative nameserver. The authoritative nameserver is usually the resolver’s last step in the journey for an IP address. The authoritative nameserver contains information specific to the domain name it serves (e.g. [google.com](http://google.com)) and it can provide a recursive resolver with the IP address of that server found in the DNS A record, or if the domain has a CNAME record it will provide the recursive resolver with an alias domain, at which point the recursive resolver will have to perform a whole new DNS lookup to procure a record from an authoritative nameserver (often an A record containing an IP address).

## What is the `dig` command and when it is used

* `dig` **(Domain Information Groper) command is a network tool with a basic command-line interface that serves for making different DNS (domain name system) queries.** You can use the DIG command to:
    
    * Diagnose your name servers. Check all of them or each individual server and their response.
        
    * Check all of the available DNS records or individual DNS records and their parameters.
        
    * Trace IP addresses and see the hostnames that correspond to them.
        
    * Do a query through a specific port that you want to use.
        
    * See the TTL value of the DNS records and know how often, do they refresh.
        
    * Trace the route of a DNS query.
        
    
    You can find the DIG command pre-installed on most Linux distros. You can easily install it on macOS, too**,** with brew, and get the DIG command on Windows 10 with bind9[.](https://www.cloudns.net/wiki/article/9/)
    

The `dig` command works by performing a DNS query from your device to the targeted IP address or hostname. The query will first arrive at your ISP’s recursive name servers. If there is your answer, it will return it fast. If not, your query will be rerouted in search of the answer. There could be another recursive DNS server that can answer the query, or it could arrive at the authoritative DNS name server, which will have the answer, and you will get your DNS query resolved.

## `dig` **command syntax**

The `dig` command is an incredibly versatile tool used for querying DNS servers. Understanding its syntax is key to unlocking its full potential. The general format of a `dig` command is as follows:

![Dig command](https://www.cloudns.net/blog/wp-content/uploads/2023/11/Dig.png align="left")

* **@server**: This is optional. Use it to specify the DNS server you want to query. If omitted, DIG uses the default server.
    
* **domain**: This is the domain name you are querying about.
    
* **query-type**: This specifies the type of DNS record you want to query (e.g., A, MX, NS). If not specified, DIG defaults to querying A records.
    
* **options**: DIG offers various options to format or filter query results. Here are some common options used with dig:
    
    * *+short*: Gives a shorter, more concise output.
        
    * *+trace*: Traces the path of the query across the DNS namespace.
        
    * *+noall +answer*: Shows only the answer section of the query.
        

### Let’s understand some `dig` commands

1. Understanding `dig . NS`
    
    **What this command does**
    
    `dig . NS` asks DNS: **Who are the name servers for the DNS root..?**
    
    The dot (`.`) represents the **root of the DNS hierarchy**.
    
    Example command
    
    ```markdown
    dig . NS
    ```
    
    ### Output
    
    ```markdown
    .     518400  IN  NS  a.root-servers.net.
    .     518400  IN  NS  b.root-servers.net.
    .     518400  IN  NS  c.root-servers.net.
    ```
    
    ### What this means
    
    * These are the **root name servers (A–M)**
        
    * Root servers do **not** store IP addresses of websites
        
    * They only know **which servers manage TLDs** like `.com`, `.org`, `.in`
        
2. Understanding `dig com NS`
    
    **What this command does:**
    
    * `dig com NS` asks DNS: **Which name servers manage the** `.com` domain..?
        

This command shows who controls all `.com` websites.

### Example command

```markdown
dig com NS
```

### Output

```markdown
com.   172800  IN  NS  a.gtld-servers.net.
com.   172800  IN  NS  b.gtld-servers.net.
```

### What this means

* These are **Top-Level Domain (TLD) servers**
    
* Managed by organizations like **Verisign**
    
* They don’t know website IPs
    
* They point to **authoritative servers for .com domains**
    

3. Understanding `dig` [`google.com`](http://google.com) `NS`
    

**What this command does**

`dig` [`google.com`](http://google.com) `NS` asks DNS: **Which servers are authoritative for** [**google.com**](http://google.com)**..?**

This command tells us who has the final authority over [google.com](http://google.com).

### Example command

```markdown
dig google.com NS
```

### Output

```markdown
google.com.  21600  IN  NS  ns1.google.com.
google.com.  21600  IN  NS  ns2.google.com.
```

### What this means

* These servers are controlled by **Google**
    
* They store actual DNS records: A / AAAA (IP addresses), MX, TXT, CNAME, etc.
    
* This is the **source of truth** for [google.com](http://google.com)
    

### Understanding `dig` [`google.com`](http://google.com)

When you type [**google.com**](http://google.com) in your browser, your computer first sends a **recursive DNS query** to a DNS resolver (usually provided by your ISP). If the resolver doesn’t already have the answer cached, it starts the lookup process by asking a **root name server** which servers handle the `.com` domain. The root server doesn’t know the IP address of [google.com](http://google.com), but it responds with the list of **.com TLD name servers**. The resolver then queries a TLD server, which replies with the **authoritative name servers for** [**google.com**](http://google.com). Next, the resolver contacts one of Google’s authoritative servers, which finally returns the **IP address** for [google.com](http://google.com). The resolver caches this IP address for future requests and sends it back to your browser. Using this IP, the browser connects directly to Google’s server and loads the website.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769685979494/16784367-3cf3-4455-870c-636274a7be5a.png align="center")

### Key Takeaways

* DNS translates **human-readable domain names** (like [`google.com`](http://google.com)) into **IP addresses** that computers can understand.
    
* DNS resolution follows a **hierarchical flow**: **Recursive Resolver → Root Server → TLD Server → Authoritative Server**.
    
* The **recursive resolver** does all the heavy lifting and caches results to improve performance.
    
* **Root name servers** sit at the top of the DNS hierarchy and only point to **TLD servers**, not IP addresses.
    
* **TLD name servers** (like `.com`) direct queries to the **authoritative name servers** for a domain.
    
* **Authoritative name servers** are the **source of truth** and return the final DNS records, such as A or CNAME.
    
* The `dig` command is a powerful tool to inspect and trace DNS resolution step by step.
    
* Commands like `dig . NS`, `dig com NS`, and `dig` [`google.com`](http://google.com) `NS` help visualize each layer of the DNS system.
    
* Even though DNS resolution looks simple to users, it involves **multiple servers working together in milliseconds**.
    

Understanding DNS and tools like `dig` helps demystify how the internet works behind the scenes and makes debugging network issues much easier.