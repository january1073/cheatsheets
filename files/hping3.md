# hping3 Cheatsheet
![Command and Control](https://img.shields.io/badge/Command%20and%20Control-DC143C?style=flat-square&logoColor=white) ![Defense Evasion](https://img.shields.io/badge/Defense%20Evasion-DC143C?style=flat-square&logoColor=white) ![Reconnaissance](https://img.shields.io/badge/Reconnaissance-DC143C?style=flat-square&logoColor=white) ![Resource Development](https://img.shields.io/badge/Resource%20Development-DC143C?style=flat-square&logoColor=white)

## Basic Information

| Description | Details |
|-------------|---------|
| Basic Syntax | hping3 [options] target |
| Requirements | Requires **root/administrator privileges** |

## Command Options

### Core Options

| Option | Description |
|--------|-------------|
| -h, --help | Show help |
| -v, --version | Show version |
| -c COUNT | Packet count (default: infinite) |
| -i INTERVAL | Wait interval between packets (default: 1 sec, use 'u' for microseconds) |
| -n, --numeric | Numeric output only, no hostname resolution |
| -q, --quiet | Quiet output |
| -V, --verbose | Verbose mode |
| -D, --debug | Debug info |
| -z | Zero checksum mode |
| -Z | Add a timestamp to each received packet |
| --beep | Beep when packet received |

### Protocol Mode Selection

| Option | Description |
|--------|-------------|
| Default | TCP mode (default protocol) |
| -1, --icmp | ICMP mode |
| -2, --udp | UDP mode |
| -8, --scan | Scan mode (TCP port scan) |
| -9, --listen | Listen mode |
| -0, --rawip | RAW IP mode |

### IP Options

| Option | Description |
|--------|-------------|
| -a, --spoof HOST | Spoof source address |
| -t TTL, --ttl TTL | Set IP time-to-live field |
| -N, --id ID | Set IP ID field |
| -W SIZE, --winid SIZE | Use win* id byte ordering |
| -r, --rel | Relativize ID field |
| -f, --frag | Set fragmentation bit |
| -x, --morefrag | Set more fragments bit |
| -y, --dontfrag | Set don't fragment bit |
| -g OFFSET, --fragoff OFFSET | Set fragment offset |
| -m MTU, --mtu MTU | Set virtual MTU |
| -o OFFSET, --tos OFFSET | Set type of service |
| -G, --rroute | Record route |
| --ipproto PROTOCOL | Set IP protocol field |
| --rand-source | Random source IP address |
| --rand-dest | Random destination IP address |

## Protocol-Specific Options

### TCP Options

| Option | Description |
|--------|-------------|
| -s PORT, --baseport PORT | Base source port (default: random) |
| -p PORT, --destport PORT | Destination port (default: 0) |
| -k, --keep | Keep source port constant |
| -w SIZE, --win SIZE | TCP window size |
| -O SIZE, --tcpoff SIZE | Set fake TCP data offset |
| -Q, --seqnum | Show only TCP sequence number |
| -b, --badcksum | Send packets with bad checksum |
| -M, --setseq SEQUENCE | Set TCP sequence number |
| -L, --setack ACK | Set TCP ACK |

### TCP Flag Options

| Option | Description |
|--------|-------------|
| -F, --fin | Set FIN flag |
| -S, --syn | Set SYN flag |
| -R, --rst | Set RST flag |
| -P, --push | Set PUSH flag |
| -A, --ack | Set ACK flag |
| -U, --urg | Set URG flag |
| -X, --xmas | Set X unused flag |
| -Y, --ymas | Set Y unused flag |

### ICMP Options

| Option | Description |
|--------|-------------|
| -C CODE, --icmpcode CODE | Set ICMP code |
| -K TYPE, --icmptype TYPE | Set ICMP type |
| --icmp-ipver VERSION | Set IP version in ICMP unreachable |
| --icmp-iphlen LENGTH | Set IP header length in ICMP unreachable |
| --icmp-iplen LENGTH | Set IP total length in ICMP unreachable |
| --icmp-ipid ID | Set IP id in ICMP unreachable |
| --icmp-ipproto PROTO | Set IP protocol in ICMP unreachable |

### UDP Options

| Option | Description |
|--------|-------------|
| -s PORT, --baseport PORT | Base source port (default: random) |
| -p PORT, --destport PORT | Destination port (default: 0) |
| --keep | Keep source port constant |

### Data Options

| Option | Description |
|--------|-------------|
| -d SIZE, --data SIZE | Data size in bytes |
| -E FILE, --file FILE | Data from file |
| -e SIGNATURE, --sign SIGNATURE | Add signature |
| -j, --dump | Dump packets in hex |
| -J, --print | Dump printable characters |
| -B, --safe | Enable safe protocol |
| -u, --end | Tell you when --file reached EOF |

### Traceroute Options

| Option | Description |
|--------|-------------|
| -T, --traceroute | Traceroute mode |
| --tr-keep-ttl | Keep TTL fixed in traceroute mode |
| --tr-stop | Exit when first packet arrived |
| --tr-no-rtt | Don't calculate/show RTT in traceroute |

## Common Use Cases

### Port Scanning

| Purpose | Command |
|---------|---------|
| TCP SYN scan (ports 1-1000) | hping3 -8 1-1000 -S target.host |
| TCP SYN scan on specific port | hping3 -S -p 80 target.host |
| UDP scan on specific port | hping3 -2 -p 53 target.host |
| Stealth FIN scan | hping3 -F -p 80 target.host |
| NULL scan (no flags) | hping3 -p 80 target.host |
| XMAS scan (all flags) | hping3 -F -S -R -P -A -U -p 80 target.host |

### Host Discovery

| Purpose | Command |
|---------|---------|
| ICMP echo (ping) | hping3 -1 target.host |
| TCP ping (SYN to port 80) | hping3 -S -p 80 -c 5 target.host |
| TCP ping with fast interval | hping3 -S -p 80 -c 5 -i u10000 target.host |
| Ping with custom data size | hping3 -1 -d 120 target.host |
| Ping sweep (with IP range) | for ip in $(seq 1 254); do hping3 -c 1 -1 192.168.1.$ip 2>/dev/null | grep "ip=" & done |

### Traceroute Techniques

| Purpose | Command |
|---------|---------|
| ICMP traceroute | hping3 --traceroute -V -1 target.host |
| TCP traceroute on port 80 | hping3 --traceroute -S -p 80 target.host |
| UDP traceroute | hping3 --traceroute -V -2 -p 53 target.host |
| Traceroute with fixed TTL | hping3 --traceroute --tr-keep-ttl -V -1 target.host |
| Faster traceroute | hping3 --traceroute -i u10000 -V -1 target.host |

### Firewall Testing

| Purpose | Command |
|---------|---------|
| Test ACK packets (firewall rule testing) | hping3 -A -p 80 target.host |
| Test FIN packets | hping3 -F -p 80 target.host |
| Test RST packets | hping3 -R -p 80 target.host |
| Test with fragmented packets | hping3 -f -p 80 target.host |
| Test with specific flags | hping3 -p 80 -S -A -P target.host |
| Test with random source IPs | hping3 --rand-source -S -p 80 target.host |

### Advanced Packet Manipulation

| Purpose | Command |
|---------|---------|
| Custom TTL manipulation | hping3 -A -t 10 -p 80 target.host |
| Custom IP ID field | hping3 -N 12345 -p 80 -S target.host |
| Send with specific payload | hping3 -d 100 -E payload.txt target.host |
| Send with specific data size | hping3 -d 120 -S -p 80 target.host |
| Set specific TCP window size | hping3 -w 4096 -S -p 80 target.host |
| Set custom TCP sequence number | hping3 -M 12345 -S -p 80 target.host |
| Set custom TCP ACK number | hping3 -L 12345 -A -p 80 target.host |

### Traffic Generation

| Purpose | Command |
|---------|---------|
| SYN flood simulation (for testing only!) | hping3 -S -p 80 --flood target.host |
| Send 1000 packets at 10 packets/sec | hping3 -c 1000 -i u100000 target.host |
| Send at specific interval | hping3 -i u50000 -S -p 80 target.host |
| Send random-sized packets | hping3 -d rand(100) target.host |
| Send with random source IPs | hping3 --rand-source -S -p 80 target.host |
| Send with randomized TTL values | hping3 -t rand(10,30) -S -p 80 target.host |

### Reconnaissance & Information Gathering

| Purpose | Command |
|---------|---------|
| OS fingerprinting via TCP options | hping3 -S -p 80 -O target.host |
| MTU discovery | hping3 -S -p 80 -y -d 1500 target.host |
| Measure host response times | hping3 -S -p 80 -c 10 -i u10000 target.host |
| Determine network path | hping3 --traceroute -V -S -p 80 target.host |
| Identify open ports | hping3 -S -p ++1 -c 1000 target.host |

### Protocol Analysis

| Purpose | Command |
|---------|---------|
| Show received packets in hex | hping3 -S -p 80 -j target.host |
| Show printable characters | hping3 -S -p 80 -J target.host |
| Show only sequence numbers | hping3 -S -p 80 -Q target.host |
| Display with timestamps | hping3 -S -p 80 -Z target.host |
| Verbose IP information | hping3 -S -p 80 -V target.host |

Reach out: https://guns.lol/january1073
