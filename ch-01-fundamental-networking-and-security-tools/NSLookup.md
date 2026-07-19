# Chapter 01 — Fundamental Networking and Security Tools

## Section 3: NSLookup

---

## What Is NSLookup?

`nslookup` stands for **Name Server Lookup**.

It is a command-line tool used to ask a DNS server for information.

You can use `nslookup` to:

- Find the IP address connected to a hostname
- Attempt to find the hostname connected to an IP address
- Find the mail servers used by a domain
- Check which DNS server answered a request
- Troubleshoot DNS problems

---

## Noninteractive Mode

Noninteractive mode performs one lookup and then returns to the normal Terminal prompt.

### Look Up a Hostname

```bash
nslookup google.com
```

This asks DNS for the IP address connected to `google.com`.

### Look Up an IP Address

```bash
nslookup 8.8.8.8
```

This performs a reverse DNS lookup.

A reverse lookup attempts to find the hostname connected to an IP address.

It may not return a hostname if a PTR record has not been configured.

### Find Mail Servers

```bash
nslookup -type=MX gmail.com
```

An **MX record** identifies the mail servers responsible for receiving email for a domain.

---

## Interactive Mode

Enter:

```bash
nslookup
```

This opens the interactive `nslookup` prompt.

You can then enter a hostname:

```text
google.com
```

To request a specific type of DNS record:

```text
set type=MX
gmail.com
```

To leave interactive mode:

```text
exit
```

---

## Understanding Basic NSLookup Output

Example:

```text
Server: 192.168.1.254
Address: 192.168.1.254#53

Non-authoritative answer:
Name: google.com
Address: 142.250.72.14
```

### Important Parts

- **Server** — the DNS server that answered the request
- **Address** — the IP address of the DNS server
- **#53** — DNS normally uses port 53
- **Non-authoritative answer** — the answer came from a caching or recursive DNS server
- **Name** — the hostname that was searched
- **Address** — the IP address returned for the hostname

---

## Useful Commands

### Find IPv4 Addresses

```bash
nslookup -type=A google.com
```

### Find IPv6 Addresses

```bash
nslookup -type=AAAA google.com
```

### Find Mail Servers

```bash
nslookup -type=MX gmail.com
```

### Find Name Servers

```bash
nslookup -type=NS google.com
```

### Perform a Reverse Lookup

```bash
nslookup 8.8.8.8
```

---

## Summary

> `nslookup` asks a DNS server for information about hostnames, IP addresses, and DNS records.
