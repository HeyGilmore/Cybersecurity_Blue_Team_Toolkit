# Chapter 01 - Fundamental Networking and Security Tools

## Ping 

* A `ping` is a command-line networking tool used to test whether another host can be reached over an IP network.
A `host` is a computer, server, printer, router, phone, or other device connected to a network.
Ping measures the `round-trip time` required for a message to travel from the source computer to the destination and back.
The name is commonly associated with the echoing sound used by sonar.

### Echolocation Comparison
* Bats use echolocation by producing sounds and listening for the echoes that return after hitting nearby objects.
* Ping works in a similar way:
    1. The source computer sends an ICMP Echo Request.
    2. The destination receives the request.
    3. The destination sends an ICMP Echo Reply.
    4. The source measures how long the round trip took.

### How Ping Works
* Ping uses the Internet Control Message Protocol, or ICMP.
* It sends an ICMP Echo Request to a hostname or IP address.
* It waits for an ICMP Echo Reply.
* It can report:
    1. Whether the destination responded
    2. Round-trip time
    3. Packet loss
    4. Certain network errors


```bash
Example:
ping www.google.com

Example output:
PING www.google.com (142.251.154.119): 56 data bytes
64 bytes from 142.251.154.119: icmp_seq=0 ttl=115 time=21.178 ms
64 bytes from 142.251.154.119: icmp_seq=1 ttl=115 time=14.289 ms
64 bytes from 142.251.154.119: icmp_seq=2 ttl=115 time=15.271 ms
```

* In this example:
    * `www.google.com` is the destination hostname.
    * `142.251.154.119` is the destination IP address returned through DNS. It is associated with the Google Server or network destination you are pinging, not your computer
    * `icmp_seq` is the sequence number of each request. It helps identify missing, duplicate, delayed, or out-of-order replies.
    * `ttl` shows the remaining Time to Live value. Each router normally decreases the TTL by one. If TTL reaches zero, the packet is discarded. The displayed TTL is the remaining value in the reply, not a direct measurement of the exact number of routers traveled.
    * `time` shows the round-trip time in milliseconds.
A successful ping means the destination responded to ICMP traffic. It confirms basic IP connectivity, but it does not prove that applications such as websites or SSH are working.Could be a firewall may be blocking ICMP traffic as well. 


### Pinging the Loopback Address
```bash
ping 127.0.0.1

For IPv6:

ping6 ::1

```
* `127.0.0.1` is a special reserved IP address, called an IPv4 loopback address.
It is also known as localhost.
Pinging it tests whether the local computer’s TCP/IP networking software is functioning.
It does not test the Wi-Fi connection, router, internet provider, or internet access.
The IPv6 loopback address is:
::1

***This is the first tool to pull out of your toolkit, when you are experiencing network difficulties. Go PING yourself and make sure everything is working as it should.***

---
### Questions I had: 

#### 1. Does the IP address always stay the same? 
* Hostname's IP address: A hostname such as www.google.com can resolve to different IP addresses. Large organizations operate many servers, and DNS may return different addresses depending on location, availability, and network conditions.
    * One hostname can have multiple addresses, and multiple hostnames can sometimes point to the same address.
* Your Private IP address: Your router assigns this to your computer, often through DHCP. It may remain the same for a while but can change.
* Your public IP address: Your internet provider assigns this to your home network. It can also change unless you pay for or configure a static IP address. 
***An IP address is not necessarily permanently attached to one device.***

#### 2. How Do I Find My Hostname?

```
 hostname
```
* A `hostname` is the name assigned to a computer or network device so that it can be identified on a network.

---

### Windows Options
| Option     | Meaning                                                          | Example                 |
| ---------- | ---------------------------------------------------------------- | ----------------------- |
| `/?`       | Displays ping help and available options                         | `ping /?`               |
| `/t`       | Continues pinging until stopped with `Ctrl + C`                  | `ping /t 8.8.8.8`       |
| `/a`       | Attempts to resolve an IP address to a hostname                  | `ping /a 8.8.8.8`       |
| `/n count` | Chooses the number of requests to send; Windows defaults to four | `ping /n 10 google.com` |
| `/r count` | Records the route for up to nine hops; IPv4 only                 | `ping /r 5 google.com`  |
| `/s count` | Records timestamps for up to four hops; IPv4 only                | `ping /s 4 google.com`  |
| `/i TTL`   | Sets the outgoing Time to Live value; maximum 255                | `ping /i 20 google.com` |



### Common Ping Options on macOS

| Option |Meaning  | Example  |
| :----------------- | :----------------- | :------------------------ |
| `man ping`         | Opens the macOS manual showing all available `ping` options   | `man ping`      |
| No option required | On macOS, ping continues until you stop it with `Control + C`   | `ping google.com`  |
| `-c count`   | Sends a specific number of echo requests and then stops   | `ping -c 4 google.com`    |
| `-a`    | Makes an audible sound when a reply is received   | `ping -a google.com`      |
| `-n`    | Displays only numeric IP addresses and does not attempt hostname lookups                                                   | `ping -n google.com`      |
| `-R`    | Historically recorded the route taken by the packet, but this option is deprecated and no longer functions on modern macOS | `ping -R google.com`      |
| `-M time`    | Sends an ICMP timestamp request instead of a normal Echo Request   | `ping -M time google.com` |
| `-m ttl`    | Sets the Time to Live value for outgoing packets     | `ping -m 64 google.com`   |
| `-i seconds` | Changes the amount of time between each ping request  | `ping -i 2 google.com`    |
| `-s bytes` | Changes the number of data bytes sent in each packet    | `ping -s 100 google.com`  |
| `-t seconds` | Stops the entire ping command after the specified number of seconds | `ping -t 10 google.com`   |
| `-W milliseconds`| Sets how long to wait for each individual reply | `ping -W 2000 google.com` |
| `-q` | Shows only the beginning and final statistics instead of every reply                                                       | `ping -q -c 4 google.com` |
| `-o`  | Stops after receiving the first successful reply   | `ping -o google.com`      |

---


## Summary of Ping
>Ping sends an ICMP Echo Request to a hostname or IP address and waits for an Echo Reply. It helps determine whether a destination is reachable, how long the round trip takes, and whether packets are being lost. A failed ping does not automatically mean the destination is offline because ICMP traffic may be blocked.
