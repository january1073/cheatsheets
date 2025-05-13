# tcpdump Cheatsheet
![Collection](https://img.shields.io/badge/Collection-DC143C?style=flat-square&logoColor=white) ![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white)

## Basic Information

| Description | Details |
|-------------|---------|
| Basic Syntax | tcpdump [options] [expression] |
| Requirements | Requires **root/administrator privileges** |

## Command Options

### Core Options

| Option | Description |
|--------|-------------|
| -i <interface> | Specify network interface to capture (e.g., eth0, wlan0) |
| -n | Don't resolve hostnames |
| -nn | Don't resolve hostnames or port names |
| -c <count> | Exit after capturing specified number of packets |
| -s <snaplen> | Capture up to snaplen bytes per packet (0 = max available) |
| -w <file> | Write raw packets to file |
| -r <file> | Read packets from file |

### Output Format Options

| Option | Description |
|--------|-------------|
| -v, -vv, -vvv | Increase verbosity level |
| -X | Show packet data in hex and ASCII |
| -XX | Show Ethernet header as well as packet data |
| -A | Print packets in ASCII |
| -e | Print link-level header on each line |
| -q | Quick output (less protocol info) |

### Timestamp Options

| Option | Description |
|--------|-------------|
| -t | Don't print timestamp |
| -tttt | Print timestamp in YYYY-MM-DD HH:MM:SS format |
| -S | Print absolute TCP sequence numbers |

## Filter Expressions

### Protocol Filters

| Expression | Description |
|------------|-------------|
| tcp | Capture only TCP packets |
| udp | Capture only UDP packets |
| icmp | Capture only ICMP packets |
| arp | Capture only ARP packets |
| ip | Capture only IPv4 packets |
| ip6 | Capture only IPv6 packets |
| http or port 80 | Capture HTTP traffic |
| https or port 443 | Capture HTTPS traffic |
| dns or port 53 | Capture DNS traffic |
| ssh or port 22 | Capture SSH traffic |

### Host & Network Filters

| Expression | Description |
|------------|-------------|
| host <hostname/IP> | Capture packets involving the specified host |
| src host <hostname/IP> | Capture packets from the specified source host |
| dst host <hostname/IP> | Capture packets to the specified destination host |
| net <network/mask> | Capture packets involving the specified network |
| src net <network/mask> | Capture packets from the specified network |
| dst net <network/mask> | Capture packets to the specified network |

### Port Filters

| Expression | Description |
|------------|-------------|
| port <number> | Capture packets involving the specified port |
| src port <number> | Capture packets from the specified source port |
| dst port <number> | Capture packets to the specified destination port |
| portrange <start>-<end> | Capture packets involving ports in the range |

### Packet Size Filters

| Expression | Description |
|------------|-------------|
| less <length> | Capture packets smaller than specified length |
| greater <length> | Capture packets larger than specified length |

### Logical Operators

| Operator | Description |
|----------|-------------|
| and or && | Logical AND |
| or or \|\| | Logical OR |
| not or ! | Logical NOT |

### TCP Flag Filters

| Expression | Description |
|------------|-------------|
| tcp[tcpflags] & tcp-syn != 0 | Capture SYN packets |
| tcp[tcpflags] & (tcp-syn\|tcp-ack) == (tcp-syn\|tcp-ack) | Capture SYN-ACK packets |
| tcp[tcpflags] & (tcp-push\|tcp-ack) == (tcp-push\|tcp-ack) | Capture PSH-ACK packets |
| tcp[tcpflags] & tcp-fin != 0 | Capture FIN packets |
| tcp[tcpflags] & tcp-rst != 0 | Capture RST packets |

## Common Use Cases

### Basic Packet Capturing

| Purpose | Command |
|---------|---------|
| Capture on default interface | tcpdump |
| Capture on specific interface | tcpdump -i eth0 |
| Capture specific number of packets | tcpdump -c 10 |
| Capture with no name resolution | tcpdump -nn |

### Traffic Monitoring by Host/Network

| Purpose | Command |
|---------|---------|
| Capture traffic from/to specific host | tcpdump host 192.168.1.100 |
| Capture traffic from specific source | tcpdump src host 192.168.1.100 |
| Capture traffic to specific destination | tcpdump dst host 192.168.1.100 |
| Capture traffic on subnet | tcpdump net 192.168.1.0/24 |

### Protocol-Specific Monitoring

| Purpose | Command |
|---------|---------|
| Capture HTTP traffic | tcpdump port http or port 80 |
| Capture HTTPS traffic | tcpdump port https or port 443 |
| Capture DNS traffic | tcpdump port domain or port 53 |
| Capture SSH traffic | tcpdump port ssh or port 22 |
| Capture SMTP traffic | tcpdump port smtp or port 25 |
| Capture all web traffic | tcpdump port 80 or port 443 |

### Advanced Filtering Techniques

| Purpose | Command |
|---------|---------|
| Traffic between two hosts on specific port | tcpdump tcp and host 192.168.1.100 and host 192.168.1.200 and port 80 |
| All traffic except SSH | tcpdump not port 22 |
| Traffic by packet size | tcpdump less 64 or tcpdump greater 500 |
| Capture only TCP SYN packets | tcpdump 'tcp[tcpflags] & tcp-syn != 0' |
| Capture SYN-ACK packets | tcpdump 'tcp[tcpflags] & (tcp-syn\|tcp-ack) == (tcp-syn\|tcp-ack)' |

### Output Control & Analysis

| Purpose | Command |
|---------|---------|
| Save captured packets to file | tcpdump -w capture.pcap |
| Read packets from file | tcpdump -r capture.pcap |
| Display packet data in hex and ASCII | tcpdump -X |
| Display packet data with link layer header | tcpdump -XX |
| Display ASCII text in packets | tcpdump -A port 80 |
| Verbose output with timestamps | tcpdump -vv -tttt |

### HTTP Request Filtering

| Purpose | Command |
|---------|---------|
| Filter HTTP GET requests | tcpdump -i eth0 -s 0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' |
| Filter HTTP POST requests | tcpdump -i eth0 -s 0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0) and (tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354)' |

### Security Monitoring Examples

| Purpose | Command |
|---------|---------|
| Detect port scanning | tcpdump -nn 'tcp[tcpflags] == tcp-syn' |
| Monitor failed connection attempts | tcpdump 'tcp[tcpflags] & (tcp-syn\|tcp-rst) == tcp-rst' |
| Detect potential DoS attack | tcpdump -nn 'tcp[tcpflags] == tcp-syn' | wc -l |
| Detect ICMP flood | tcpdump -n icmp and icmp[icmptype]==icmp-echo |
| Monitor DNS queries | tcpdump -i eth0 udp port 53 and not src host 192.168.1.1 |

### Performance Optimization

| Purpose | Command |
|---------|---------|
| Limit captured data per packet | tcpdump -s 96 port 80 |
| Fast capture with minimal processing | tcpdump -nn -q -s 0 -i eth0 |
| Write to file without processing | tcpdump -nn -q -s 0 -i eth0 -w capture.pcap |
| Split captures into multiple files | tcpdump -w capture_%Y-%m-%d_%H:%M:%S.pcap -G 3600 |
| Rotate capture files | tcpdump -w capture.pcap -C 1 -W 10 |

Reach out: https://linktr.ee/january1073
