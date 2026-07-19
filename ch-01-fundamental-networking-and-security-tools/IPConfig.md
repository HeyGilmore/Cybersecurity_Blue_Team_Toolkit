# Chapter 01 — Fundamental Networking and Security Tools

## Section 2: IP Configuration

---

## What Is IP?

**IP** stands for **Internet Protocol**.

IP helps move data from one device to another across a network.

It uses:

- A **source IP address** — where the packet came from
- A **destination IP address** — where the packet is going

IP helps send packets toward the correct destination, but it does not guarantee that every packet will arrive.

---

## What Is IP Configuration?

**IP configuration** is the network information a device needs to communicate with other devices and connect to the internet.

It can include:

- **IP address** — identifies the device on the network
- **Subnet mask** — helps determine which devices are on the same local network
- **Default gateway** — usually the router that sends traffic to other networks
- **DNS servers** — translate domain names into IP addresses
- **Network interface** — the connection being used, such as Wi-Fi or Ethernet
- **DHCP status** — shows whether the network settings were assigned automatically
- **DHCP lease** — shows how long the device may use its assigned IP address

---

## What Is `ipconfig`?

`ipconfig` is a Windows command used to view and manage network configuration.

Example on Windows:

```powershell
ipconfig /all
```

macOS uses several different commands for similar tasks:

```bash
ifconfig
```

Displays network interfaces and IP addresses.

```bash
networksetup -getinfo Wi-Fi
```

Displays information about the Wi-Fi connection.

```bash
scutil --dns
```

Displays the Mac's DNS configuration.

macOS also has a command named `ipconfig`, but it is different from the Windows version.

---

# Packets

## What Is a Packet?

A **packet** is a small unit of data sent across a network.

An IP packet has two main parts:

1. **Header**
2. **Payload**

---

## Header

The **header** contains information needed to deliver and process the packet.

It may include:

- Source IP address
- Destination IP address
- Packet length
- Protocol information
- Time to Live, or TTL

### Simple comparison

The header is like the address and shipping label on a package.

---

## Payload

The **payload** is the actual information being carried inside the packet.

It may contain:

- Part of a website
- Part of an email or message
- Part of a downloaded file
- A ping request or reply
- Data sent by an application

### Simple comparison

The payload is like the item inside a package.

---

# Viewing Network Information on macOS

## Display All Network Interfaces

```bash
ifconfig
```

This command displays information about the Mac's network interfaces.

Common interface names include:

- `en0`
- `en1`
- `lo0`

`lo0` is the loopback interface.

The Wi-Fi interface is often `en0`, but this can vary between Macs.

---

## Identify Network Hardware Ports

```bash
networksetup -listallhardwareports
```

This command helps identify which interface belongs to Wi-Fi or Ethernet.

Example:

```text
Hardware Port: Wi-Fi
Device: en0
```

In this example, the Wi-Fi interface is `en0`.

---

## Display the IPv4 Address of an Interface

```bash
ipconfig getifaddr en0
```

This displays the IPv4 address assigned to `en0`.

Replace `en0` if your Mac uses a different interface.

---

# Default Gateway

## What Is a Default Gateway?

The **default gateway** is usually the router.

Your Mac sends packets to the default gateway when the destination is outside your local network.

### Simple example

```text
Mac → Router → Internet
```

---

## Find the Router's Private IP Address

```bash
route -n get default | grep gateway
```

Example output:

```text
gateway: 192.168.1.254
```

In this example, `192.168.1.254` is the router's private IP address.

You can also find it through:

```text
System Settings
→ Network
→ Wi-Fi
→ Details
→ TCP/IP
→ Router
```

---

# DNS

## What Is DNS?

**DNS** stands for **Domain Name System**.

DNS translates human-readable domain names into IP addresses.

Example:

```text
google.com → 142.x.x.x
```

DNS allows people to remember names such as `google.com` instead of numerical IP addresses.

---

## DNS Cache

A **DNS cache** temporarily stores the results of previous DNS lookups.

This can:

- Speed up repeated requests
- Reduce the number of DNS queries
- Reduce traffic sent to DNS servers

A DNS cache is not a complete list of every website visited.

It may contain lookups made by:

- Websites
- Applications
- Background services
- The operating system

---

## Display DNS Configuration on macOS

```bash
scutil --dns
```

This command shows how the Mac is configured to perform DNS lookups.

Important parts of the output may include:

- **nameserver** — the DNS server being used
- **en0** — the network interface using the DNS settings
- **A record** — returns an IPv4 address
- **AAAA record** — returns an IPv6 address
- **Reachable** — the Mac can communicate with the DNS server
- **mDNS** — helps find local devices ending in `.local`

This command does not show browser history.

---

## Look Up a Specific Hostname on macOS

```bash
dscacheutil -q host -a name google.com
```

This asks macOS for the IP addresses associated with `google.com`.

The output may include:

- `ip_address` — IPv4 address
- `ipv6_address` — IPv6 address

One hostname may return several valid IP addresses.

---

## Query a DNS Server

```bash
nslookup google.com
```

Important parts of the output:

- **Server** — the DNS server that answered
- **Address** — the DNS server's address
- **#53** — DNS normally uses port 53
- **Non-authoritative answer** — the answer came from a recursive or caching DNS server
- **Name** — the hostname that was searched
- **Address** — an IP address returned for the hostname

Different DNS tools may return different valid IP addresses because of caching and server selection.

---

## DNS Cache Poisoning

**DNS cache poisoning**, also called **DNS spoofing**, happens when false DNS information is placed into a DNS cache.

Example:

1. A user enters the correct website name.
2. The poisoned DNS information returns the wrong IP address.
3. The user is sent to a malicious destination.

**DNSSEC** can help protect DNS information by checking its authenticity and integrity.

---

# DHCP

## What Is DHCP?

**DHCP** stands for **Dynamic Host Configuration Protocol**.

- **Dynamic** — information can be assigned and changed automatically
- **Host** — a device connected to a network
- **Configuration** — the settings the device needs
- **Protocol** — rules that allow devices to communicate

DHCP automatically provides devices with network settings.

These settings may include:

- Private IP address
- Subnet mask
- Default gateway
- DNS servers
- Lease duration

---

## DHCP Server

A **DHCP server** assigns network settings to devices called clients.

On a home network, the router usually acts as the DHCP server.

The router normally:

1. Receives a public IP address from the Internet Service Provider, or ISP
2. Assigns private IP addresses to devices inside the home network

In a large organization, dedicated servers may provide DHCP services.

---

## DHCP Lease

A DHCP-assigned IP address is usually provided for a limited amount of time called a **lease**.

The DHCP server determines:

- Which device receives an address
- Which address is assigned
- How long the device may use it
- Whether the lease can be renewed

Renewing a lease does not guarantee a different IP address. The DHCP server may assign the same address again.

---

## Renew a DHCP Lease on macOS

Use:

```text
System Settings
→ Network
→ Wi-Fi
→ Details
→ TCP/IP
→ Renew DHCP Lease
```

This asks the DHCP server to renew the Mac's network configuration.

---

# Windows and macOS Command Comparison

| Purpose | Windows | macOS |
|---|---|---|
| Show network configuration | `ipconfig /all` | `ifconfig` |
| Show Wi-Fi information | `ipconfig /all` | `networksetup -getinfo Wi-Fi` |
| Show DNS configuration | `ipconfig /all` | `scutil --dns` |
| Display DNS cache | `ipconfig /displaydns` | No exact equivalent |
| Look up one hostname | `nslookup google.com` | `dscacheutil -q host -a name google.com` |
| Query DNS | `nslookup google.com` | `nslookup google.com` |
| Flush DNS-related cache | `ipconfig /flushdns` | `sudo dscacheutil -flushcache` |
| Release DHCP address | `ipconfig /release` | No normal direct equivalent |
| Renew DHCP lease | `ipconfig /renew` | Use **Renew DHCP Lease** in System Settings |
| Show default gateway | `ipconfig` | `route -n get default` |
| Show one interface's IPv4 address | `ipconfig` | `ipconfig getifaddr en0` |

---

# Summary
> IP moves packets between devices. DNS finds IP addresses for names, DHCP provides network settings, and the default gateway sends traffic outside the local network.

- **IP** addresses and delivers packets between devices.
- **IP configuration** contains the settings a device needs to communicate.
- A **packet** contains a header and a payload.
- The **default gateway** sends traffic outside the local network.
- **DNS** translates domain names into IP addresses.
- **DHCP** automatically provides devices with network settings.
- A **DHCP lease** controls how long a device may use an assigned IP address.
- Windows mainly uses `ipconfig`.
- macOS uses several commands, including `ifconfig`, `networksetup`, `scutil`, and `ipconfig`.


