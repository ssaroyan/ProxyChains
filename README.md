# ProxyChains Guide (SOCKS Proxy on Linux)

Overview
--------
ProxyChains is a powerful Linux tool that forces any TCP-based application to route its network traffic through a proxy server.

It is especially useful when:
- Applications do not support proxy settings natively
- You need to bypass network restrictions
- You want to use a SOCKS5 proxy for tools like apt, curl, git, etc.

ProxyChains works by intercepting network system calls and redirecting connections through configured proxies.

Installation
------------
Install ProxyChains on Ubuntu/Debian:

sudo apt update
sudo apt install proxychains4 -y

Check installation:

proxychains --version

Configuration
-------------
ProxyChains reads its configuration from:

/etc/proxychains.conf

Open the file:

sudo nano /etc/proxychains.conf

Scroll to the bottom and locate the section:

[ProxyList]

Add your proxy entry below it.

Example SOCKS5 Proxy:

[ProxyList]
socks5  37.221.17.32 2222

Example Local SSH SOCKS Proxy:

ssh -D 1080 root@YOUR_SERVER_IP

Then configure:

[ProxyList]
socks5  127.0.0.1 1080

Basic Usage
-----------
To proxy any command, simply prepend proxychains:

proxychains <command>

Examples:

Curl through proxy:
proxychains curl https://example.com

Git clone through proxy:
proxychains git clone https://github.com/user/repo.git

Apt update through proxy:
proxychains sudo apt update

Installing packages through proxy:
proxychains sudo apt install docker.io

Testing Proxy Connection
------------------------
Before using apt, test whether the proxy works:

proxychains curl -I https://download.docker.com

If working, you should receive a normal HTTP response like:
HTTP/1.1 200 OK

Limitations
-----------
ProxyChains only supports TCP traffic.

It does NOT work with:
- ping (ICMP protocol)
- UDP-based applications
- Some applications using custom network stacks

Example (will not work):
proxychains ping 8.8.8.8

Disabling ProxyChains
---------------------
ProxyChains is not persistent unless explicitly used.

Run commands normally without proxychains:

sudo apt update

Removing Proxy Entry
--------------------
Edit config file:

sudo nano /etc/proxychains.conf

Comment out proxy line:

#socks5  37.221.17.32 2222

Uninstall ProxyChains
---------------------
If no longer needed:

sudo apt remove proxychains4 -y

Summary
-------
ProxyChains is a practical solution for routing Linux application traffic through SOCKS proxies, especially in restricted networks.

Usage:
proxychains <your-command>

