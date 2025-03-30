# `nmap` network scanning
[https://nmap.org/](https://nmap.org/)

```shell
# Scan single IP
nmap 192.168.1.1

# Scan multiple IPs
nmap 192.168.1.1 192.168.1.2

# Scan range
nmap 192.168.1.1-10

# Scan CIDR block
nmap 192.168.1.0/24

# Scan single port
nmap 192.168.1.1 -p 80

# Scan port range
nmap 192.168.1.1 -p 21-100

# Scan top x most commonly used ports
nmap 192.168.1.1 -top-ports 2000

# Scan all 65535 ports (may take long)
nmap 192.168.1.1 -p0-

# UDP scan
nmap 192.168.1.1 -sU
```