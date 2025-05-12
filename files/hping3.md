# hping3 Cheatsheet

## Basic syntax
```
hping3 [options] target
```

* Running `hping3` requires **root/administrator privileges**

## General options
| Option | Description |
|--------|-------------|
| `-h`, `--help` | Show help |
| `-v`, `--version` | Show version |
| `-c COUNT` | Packet count (default: infinite) |
| `-i INTERVAL` | Wait interval between packets (default: 1 sec, use 'u' for microseconds) |
| `-n`, `--numeric` | Numeric output only, no hostname resolution |
| `-q`, `--quiet` | Quiet output |
| `-V`, `--verbose` | Verbose mode |
| `-D`, `--debug` | Debug info |
| `-z` | Zero checksum mode |
| `-Z` | Add a timestamp to each received packet |
| `--rand-source` | Random source IP address |
| `--rand-dest` | Random destination IP address |
| `--beep` | Beep when packet received |

## IP options
| Option | Description |
|--------|-------------|
| `-a`, `--spoof HOST` | Spoof source address |
| `-t TTL`, `--ttl TTL` | Set IP time-to-live field |
| `-N`, `--id ID` | Set IP ID field |
| `-W SIZE`, `--winid SIZE` | Use win* id byte ordering |
| `-r`, `--rel` | Relativize ID field |
| `-f`, `--frag` | Set fragmentation bit |
| `-x`, `--morefrag` | Set more fragments bit |
| `-y`, `--dontfrag` | Set don't fragment bit |
| `-g OFFSET`, `--fragoff OFFSET` | Set fragment offset |
| `-m MTU`, `--mtu MTU` | Set virtual MTU |
| `-o OFFSET`, `--tos OFFSET` | Set type of service |
| `-G`, `--rroute` | Record route |
| `--ipproto PROTOCOL` | Set IP protocol field |

## ICMP options
| Option | Description |
|--------|-------------|
| `-1`, `--icmp` | ICMP mode (default mode is TCP) |
| `-C CODE`, `--icmpcode CODE` | Set ICMP code |
| `-K TYPE`, `--icmptype TYPE` | Set ICMP type |
| `--icmp-ipver VERSION` | Set IP version in ICMP unreachable |
| `--icmp-iphlen LENGTH` | Set IP header length in ICMP unreachable |
| `--icmp-iplen LENGTH` | Set IP total length in ICMP unreachable |
| `--icmp-ipid ID` | Set IP id in ICMP unreachable |
| `--icmp-ipproto PROTO` | Set IP protocol in ICMP unreachable |

## UDP options
| Option | Description |
|--------|-------------|
| `-2`, `--udp` | UDP mode |
| `--keep` | Keep source port constant |
| `-s PORT`, `--baseport PORT` | Base source port (default: random) |
| `-p PORT`, `--destport PORT` | Destination port (default: 0) |

## TCP options
| Option | Description |
|--------|-------------|
| `-8`, `--scan` | Scan mode, TCP port scan |
| `-9`, `--listen` | Listen mode |
| `-0`, `--rawip` | RAW IP mode |
| `-s PORT`, `--baseport PORT` | Base source port (default: random) |
| `-p PORT`, `--destport PORT` | Destination port (default: 0) |
| `-k`, `--keep` | Keep source port constant |
| `-w SIZE`, `--win SIZE` | TCP window size |
| `-O SIZE`, `--tcpoff SIZE` | Set fake TCP data offset |
| `-Q`, `--seqnum` | Show only TCP sequence number |
| `-b`, `--badcksum` | Send packets with bad checksum |
| `-M`, `--setseq SEQUENCE` | Set TCP sequence number |
| `-L`, `--setack ACK` | Set TCP ACK |
| `-F`, `--fin` | Set FIN flag |
| `-S`, `--syn` | Set SYN flag |
| `-R`, `--rst` | Set RST flag |
| `-P`, `--push` | Set PUSH flag |
| `-A`, `--ack` | Set ACK flag |
| `-U`, `--urg` | Set URG flag |
| `-X`, `--xmas` | Set X unused flag |
| `-Y`, `--ymas` | Set Y unused flag |

## Data options
| Option | Description |
|--------|-------------|
| `-d SIZE`, `--data SIZE` | Data size in bytes |
| `-E FILE`, `--file FILE` | Data from file |
| `-e SIGNATURE`, `--sign SIGNATURE` | Add signature |
| `-j`, `--dump` | Dump packets in hex |
| `-J`, `--print` | Dump printable characters |
| `-B`, `--safe` | Enable safe protocol |
| `-u`, `--end` | Tell you when --file reached EOF |
| `-T`, `--traceroute` | Traceroute mode |
| `--tr-keep-ttl` | Keep TTL fixed in traceroute mode |
| `--tr-stop` | Exit when first packet arrived |
| `--tr-no-rtt` | Don't calculate/show RTT in traceroute |

## Common use cases

### Port scanning
```
# TCP SYN scan on ports 1-1000
hping3 -8 1-1000 -S target.host

# UDP scan on specific ports
hping3 -2 -p 53 target.host
```

### Ping and traceroute
```
# Simple ping (ICMP echo)
hping3 -1 target.host

# Traceroute
hping3 --traceroute -V -1 target.host

# TCP traceroute on port 80
hping3 --traceroute -S -p 80 target.host
```

### Firewall testing
```
# Test ACK packets
hping3 -A -p 80 target.host

# Test FIN packets
hping3 -F -p 80 target.host
```

### DoS testing
```
# SYN flood (only for testing with permission!)
hping3 -S -p 80 --flood target.host
```

### Advanced tests
```
# TCP ACK scan with TTL manipulation
hping3 -A -t 10 -p 80 target.host

# Set specific TCP flags
hping3 -p 80 -S -A -P target.host

# Send with specific payload
hping3 -d 100 -E payload.txt target.host
```

### Traffic generation
```
# Send 1000 packets at 10 packets/sec
hping3 -c 1000 -i u100000 target.host

# Send random-sized packets
hping3 --rand-source -d 120 target.host
```
