[ <a href="https://github.com/january1073/cheatsheets/tree/main">Cheatsheets</a> ]

# Wireshark Security-Focused Display Filters

| Category | Filter Expression | Description |
|----------|-------------------|-------------|
| **SSH Attacks** | `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22` | Potential SSH password brute force - identifies SYN packets to SSH without corresponding ACK, indicating connection attempts |
| | `ssh.message_code == 51` | SSH authentication failures |
| **SMB/NTLM Authentication** | `ntlmssp.auth.username` | Shows NTLM authentication attempts with usernames, useful for tracking login attempts |
| | `smb.cmd == 0x73` | SMB Session Setup requests (often used for authentication) |
| | `smb2.cmd == 1` | SMB2 Session Setup requests |
| **HTTP Authentication** | `http.authorization contains "Basic"` | HTTP Basic Authentication attempts (credentials are base64 encoded) |
| | `http.authorization contains "NTLM"` | HTTP NTLM Authentication attempts |
| | `http.www_authenticate` | HTTP authentication challenges from servers |
| **SSL/TLS Issues** | `ssl.alert` | SSL/TLS alert messages, which may indicate certificate problems or other SSL issues |
| | `ssl.handshake.certificate` | SSL certificate exchanges, useful for certificate analysis |
| | `ssl.handshake.type == 2 and !(tcp.port == 443)` | SSL certificates on non-standard ports |
| **DNS Anomalies** | `dns and not udp.port == 53` | DNS traffic on non-standard ports - potential DNS tunneling or covert channel |
| | `dns.qry.type == 16` | DNS TXT queries, often used in data exfiltration |
| | `dns.flags.rcode != 0` | DNS error responses |
| **Malware & C2** | `http.user_agent contains "curl"` | Basic HTTP requests potentially from scripts rather than browsers |
| | `tcp.port == 4444` | Default Metasploit handler port (often used in pentests) |
| | `ip.ttl < 10` | Suspiciously low TTL values (potential covert channel) |
| **Port Scanning** | `tcp.flags.syn == 1 and tcp.flags.ack == 0` | SYN scan (typical port scanning technique) |
| | `tcp.flags.fin == 1 and tcp.flags.ack == 0` | FIN scan (stealthy port scanning) |
| | `tcp.flags == 0x29` | XMAS scan (FIN, PSH, URG flags set) |
| **ARP Spoofing** | `arp.duplicate-address-detected` | Duplicate IP addresses in ARP (potential ARP spoofing) |
| | `arp.src.hw_mac != eth.src` | MAC address inconsistency between ARP and Ethernet |
| **SMB Exploits** | `smb.cmd == 37` | SMB Trans2 commands (used in many SMB exploits) |
| | `dcerpc.cn_call_id == 0x1a` | Often used in MS17-010 (EternalBlue) exploitation |
| **File Transfers** | `ftp-data` | FTP data channel transfers |
| | `http.request.method == "POST"` | HTTP POST requests (potential data uploads) |
| | `http.content_type contains "multipart/form-data"` | File uploads over HTTP |
| **DoS Attacks** | `tcp.flags.syn == 1 and tcp.flags.ack == 0 and ip.dst == X.X.X.X` | SYN flood against specific target |
| | `icmp.type == 8 and ip.dst == X.X.X.X` | ICMP Echo flood (ping flood) |
| **Data Exfiltration** | `dns.resp.len > 100` | Unusually large DNS responses |
| | `http.request.uri contains ".php?"` | Potential web shell activity |
| | `icmp.type == 8 and data.len > 64` | Oversized ping packets (potential ICMP tunneling) |

## Tips for Using These Filters

1. **Combine filters** with logical operators (`and`, `or`, `not`) for more powerful analysis
2. **Use IP filters** to focus on specific hosts: `ip.addr == 192.168.1.1`
3. **Isolate conversations** with `tcp.stream eq X` where X is the stream number
4. **Create filter buttons** in Wireshark's interface for frequently used expressions
5. **Save complex filters** to filter files for reuse across captures

## Example Combined Filters

- External SSH brute force: `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22 and ip.src != 192.168.0.0/16`
- Suspicious DNS exfiltration: `dns.qry.name.len > 50 and dns.qry.type == 16`
- Password spraying detection: `ntlmssp.auth.username and (ntlmssp.ntlmv2.response.len > 0 or ntlmssp.auth.lmresponse.len > 0)`
- Clear-text credentials: `http.request and (http.authorization or http.proxy_authorization)`

[ <a href="https://github.com/january1073/cheatsheets/tree/main">Cheatsheets</a> ]
