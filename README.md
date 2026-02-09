# ProxyChains Guide (SOCKS Proxy on Linux)

## Overview

**ProxyChains** is a powerful Linux tool that forces any TCP-based application to route its network traffic through a proxy server.

It is especially useful when:

- Applications do not support proxy settings natively  
- You need to bypass network restrictions  
- You want to use a SOCKS5 proxy for tools like `apt`, `curl`, `git`, etc.

ProxyChains works by intercepting network system calls and redirecting connections through configured proxies.

---

## Installation

Install ProxyChains on Ubuntu/Debian:

```bash
sudo apt update
sudo apt install proxychains4 -y
```

Check installation:

```bash
proxychains --version
```

---

## Configuration

ProxyChains reads its configuration from:

```text
/etc/proxychains4.conf
```

Open the file:

```bash
sudo nano /etc/proxychains4.conf
```

Scroll to the bottom and locate:

```text
[ProxyList]
```

Add your proxy entry **below** it.

### Example SOCKS5 Proxy

```text
[ProxyList]
socks5  37.221.17.32 2222
```

### Example Local SSH SOCKS Proxy

Create a local SOCKS proxy with SSH:

```bash
ssh -D 1080 root@YOUR_SERVER_IP
```

Then configure:

```text
[ProxyList]
socks5  127.0.0.1 1080
```

---

## Basic Usage

To proxy any command, simply prepend `proxychains`:

```bash
proxychains <command>
```

Examples:

### Curl through proxy

```bash
proxychains curl https://example.com
```

### Git clone through proxy

```bash
proxychains git clone https://github.com/user/repo.git
```

### Apt update through proxy

```bash
proxychains sudo apt update
```

### Installing packages through proxy

```bash
proxychains sudo apt install docker.io
```

---

## Testing Proxy Connection

Before using `apt`, you can test whether the proxy works:

```bash
proxychains curl -I https://download.docker.com
```

If the proxy is working, you should receive a normal HTTP response such as:

```text
HTTP/1.1 200 OK
```

---

## Limitations

ProxyChains only supports **TCP traffic**.

It does NOT work with:

- `ping` (ICMP protocol)  
- UDP-based applications  
- Some applications using low-level custom network stacks  

Example (will not work):

```bash
proxychains ping 8.8.8.8
```

---

## Disabling ProxyChains

ProxyChains is not persistent unless explicitly used.

Run commands normally without it:

```bash
sudo apt update
```

instead of:

```bash
proxychains sudo apt update
```

---

## Removing Proxy Entry

Edit the configuration file:

```bash
sudo nano /etc/proxychains4.conf
```

Comment out your proxy line:

```text
#socks5  37.221.17.32 2222
```

---

## Uninstall ProxyChains

If you no longer need it:

```bash
sudo apt remove proxychains4 -y
```

---

## Summary

ProxyChains is a practical solution for routing Linux application traffic through SOCKS proxies, especially in restricted networks.

Usage pattern:

```bash
proxychains <your-command>
```
