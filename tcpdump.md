# Cheatsheet: tcpdump

## Syntax
```
tcpdump [options] [expression]
```

## Core Options

| Option | Description |
|--------|-------------|
| `-i <interface>` | Specify network interface to capture (e.g., eth0, wlan0) |
| `-n` | Don't resolve hostnames |
| `-nn` | Don't resolve hostnames or port names |
| `-c <count>` | Exit after capturing specified number of packets |
| `-v`, `-vv`, `-vvv` | Increase verbosity level |
| `-X` | Show packet data in hex and ASCII |
| `-XX` | Show Ethernet header as well as packet data |
| `-e` | Print link-level header on each line |
| `-S` | Print absolute TCP sequence numbers |
| `-w <file>` | Write raw packets to file |
| `-r <file>` | Read packets from file |
| `-A` | Print packets in ASCII |
| `-s <snaplen>` | Capture up to snaplen bytes per packet (0 = max available) |
| `-t` | Don't print timestamp |
| `-tttt` | Print timestamp in YYYY-MM-DD HH:MM:SS format |
| `-q` | Quick output (less protocol info) |

## Filter Expressions

### Protocol Filters
| Expression | Description |
|------------|-------------|
| `tcp` | Capture only TCP packets |
| `udp` | Capture only UDP packets |
| `icmp` | Capture only ICMP packets |
| `arp` | Capture only ARP packets |
| `ip` | Capture only IPv4 packets |
| `ip6` | Capture only IPv6 packets |

### Host & Network Filters
| Expression | Description |
|------------|-------------|
| `host <hostname/IP>` | Capture packets involving the specified host |
| `src host <hostname/IP>` | Capture packets from the specified source host |
| `dst host <hostname/IP>` | Capture packets to the specified destination host |
| `net <network/mask>` | Capture packets involving the specified network |
| `src net <network/mask>` | Capture packets from the specified network |
| `dst net <network/mask>` | Capture packets to the specified network |

### Port Filters
| Expression | Description |
|------------|-------------|
| `port <number>` | Capture packets involving the specified port |
| `src port <number>` | Capture packets from the specified source port |
| `dst port <number>` | Capture packets to the specified destination port |
| `portrange <start>-<end>` | Capture packets involving ports in the range |

### Packet Size Filters
| Expression | Description |
|------------|-------------|
| `less <length>` | Capture packets smaller than specified length |
| `greater <length>` | Capture packets larger than specified length |

### Logical Operators
| Operator | Description |
|----------|-------------|
| `and` or `&&` | Logical AND |
| `or` or `\|\|` | Logical OR |
| `not` or `!` | Logical NOT |

## Common Examples

### Basic Packet Capturing
```bash
# Capture packets on default interface
tcpdump

# Capture packets on specific interface
tcpdump -i eth0

# Capture specific number of packets
tcpdump -c 10
```

### Traffic Monitoring
```bash
# Capture traffic from specific host
tcpdump host 192.168.1.100

# Capture traffic from specific source
tcpdump src host 192.168.1.100

# Capture traffic to specific destination
tcpdump dst host 192.168.1.100

# Capture traffic involving specific port
tcpdump port 80

# Capture HTTP traffic
tcpdump port http or port 80

# Capture HTTPS traffic
tcpdump port https or port 443

# Capture DNS traffic
tcpdump port domain or port 53

# Capture SSH traffic
tcpdump port ssh or port 22
```

### Advanced Filtering
```bash
# Capture TCP traffic between two hosts on specific port
tcpdump tcp and host 192.168.1.100 and host 192.168.1.200 and port 80

# Capture all traffic except SSH
tcpdump not port 22

# Capture packets with specific TCP flags (SYN)
tcpdump 'tcp[tcpflags] & tcp-syn != 0'

# Capture packets with specific TCP flags (SYN-ACK)
tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-ack) == (tcp-syn|tcp-ack)'

# Capture TCP packets with PSH and ACK flags set
tcpdump 'tcp[tcpflags] & (tcp-push|tcp-ack) == (tcp-push|tcp-ack)'

# Capture traffic by packet size
tcpdump less 64
tcpdump greater 500
```

### Output Control & Saving
```bash
# Save captured packets to file
tcpdump -w capture.pcap

# Read packets from file
tcpdump -r capture.pcap

# Display packet data in hex and ASCII
tcpdump -X

# Display packet data including link layer header
tcpdump -XX

# Display ASCII text in packets (useful for HTTP)
tcpdump -A port 80
```

### Practical Targeting Examples
```bash
# Capture only TCP SYN packets
tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0'

# Filter HTTP GET requests
tcpdump -i eth0 -s 0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# Filter HTTP POST requests
tcpdump -i eth0 -s 0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0) and (tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354)'
```

## Security Considerations
- Running tcpdump requires root/administrator privileges
- Silent capturing can be done by redirecting output to /dev/null:
  ```bash
  tcpdump -i eth0 -w capture.pcap >/dev/null 2>&1
  ```
- Capture files can be analyzed later with tools like Wireshark
- Be aware of legal and ethical implications when capturing network traffic

## Tips
- Always use `-n` for faster performance when hostname resolution isn't needed
- Use `-s 0` to capture full packets (default is 262144 bytes)
- Use `-c` to limit capture size when testing filters
- Combine multiple expressions for precise targeting
- Use `-v`, `-vv`, or `-vvv` for increasing levels of detail
