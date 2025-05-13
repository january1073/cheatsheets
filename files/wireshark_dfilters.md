# Wireshark Display Filters (Examples)
![Collection](https://img.shields.io/badge/Collection-DC143C?style=flat-square&logoColor=white) ![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white)

## Authentication & Credentials

### HTTP Authentication

| Purpose | Display Filter |
|---------|---------------|
| Basic auth | http.authorization contains "Basic" |
| Clear-text credentials | http.request and (http.authorization or http.proxy_authorization) |
| NTLM auth | http.authorization contains "NTLM" |
| WWW authentication headers | http.www_authenticate |

### SMB Authentication

| Purpose | Display Filter |
|---------|---------------|
| NTLM authentication | ntlmssp.auth.username |
| SMB session setup | smb.cmd == 0x73 |
| SMBv2 session setup | smb2.cmd == 1 |

### SMTP Authentication

| Purpose | Display Filter |
|---------|---------------|
| SMTP login attempts | smtp.auth.username |

### SSH Authentication

| Purpose | Display Filter |
|---------|---------------|
| SSH auth failures | ssh.message_code == 51 |

## Data Exfiltration

| Purpose | Display Filter |
|---------|---------------|
| FTP data transfers | ftp-data |
| HTTP file uploads | http.content_type contains "multipart/form-data" |
| HTTP POST requests | http.request.method == "POST" |

## DNS Analysis

| Purpose | Display Filter |
|---------|---------------|
| DNS on non-standard port | dns and not udp.port == 53 |
| DNS response errors | dns.flags.rcode != 0 |
| DNS TXT queries | dns.qry.type == 16 |
| Large DNS responses | dns.resp.len > 100 |
| Long DNS query names | dns.qry.name.len > 50 and dns.qry.type == 16 |
| Suspicious DNS names | dns.qry.name matches "[a-z0-9]{20,}" |

## Lateral Movement

| Purpose | Display Filter |
|---------|---------------|
| DCERPC service control | dcerpc.cn_iface == "367abb81-9844-35f1-ad32-98f038001003" |
| SMB IPC$ access | smb.tree_share == "IPC$" |

## Malware & Command and Control

| Purpose | Display Filter |
|---------|---------------|
| Beaconing behavior | frame.time_delta < 60 and frame.time_delta > 55 |
| ICMP tunneling | icmp.type == 8 and data.len > 64 |
| Known malware ports | tcp.port in {4444, 5554, 6667, 12345, 31337} |
| Low TTL values | ip.ttl < 10 |
| Specific Metasploit port | tcp.port == 4444 |
| Tor traffic | ip.geoip.country == "A1" |

## Network Reconnaissance

### ARP Analysis

| Purpose | Display Filter |
|---------|---------------|
| ARP spoofing detection | arp.duplicate-address-detected |
| MAC address mismatch | arp.src.hw_mac != eth.src |

### LDAP/Active Directory Recon

| Purpose | Display Filter |
|---------|---------------|
| LDAP bind requests | ldap.message == 0 or ldap.bindRequest |
| LDAP search requests | ldap.searchRequest |

### Service Discovery

| Purpose | Display Filter |
|---------|---------------|
| SSL certificates | ssl.handshake.certificate |
| SSL on non-standard ports | ssl.handshake.type == 2 and !(tcp.port == 443) |
| SSL/TLS alerts | ssl.alert |

## Protocol Exploits

| Purpose | Display Filter |
|---------|---------------|
| EternalBlue exploit | dcerpc.cn_call_id == 0x1a |
| SMB Trans2 exploits | smb.cmd == 37 |
| Suspicious SMB errors | smb.flags.response == 0 and smb.flags.error_code != 0 |

## Scanning Techniques

### Password Attacks

| Purpose | Display Filter |
|---------|---------------|
| Password spray | ntlmssp.auth.username and (ntlmssp.ntlmv2.response.len > 0 or ntlmssp.auth.lmresponse.len > 0) |

### Port Scanning

| Purpose | Display Filter |
|---------|---------------|
| FIN scan | tcp.flags.fin == 1 and tcp.flags.ack == 0 |
| SYN scan | tcp.flags.syn == 1 and tcp.flags.ack == 0 |
| XMAS scan | tcp.flags == 0x29 |

### SSH Brute Force

| Purpose | Display Filter |
|---------|---------------|
| External SSH brute force | tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22 and ip.src != 192.168.0.0/16 |
| SSH brute force | tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22 |

## Suspicious Traffic Patterns

| Purpose | Display Filter |
|---------|---------------|
| ICMP flood | icmp.type == 8 and ip.dst == X.X.X.X |
| SYN flood | tcp.flags.syn == 1 and tcp.flags.ack == 0 and ip.dst == X.X.X.X |
| Suspicious SMTP attachments | smtp.attachment.filename matches "\\.(exe\|js\|scr\|vbs\|jar)$" |

## Web Application Analysis

### Common Attack Indicators

| Purpose | Display Filter |
|---------|---------------|
| PHP parameter injection | http.request.uri contains ".php?" |
| WebShell activity | http.request.uri contains "cmd=" or http.request.uri contains "exec=" or http.request.uri contains "shell=" |

### Malicious Downloads

| Purpose | Display Filter |
|---------|---------------|
| Base64 encoded content | http.request.uri contains "base64" |
| Executable downloads | http.content_type contains "application/x-msdownload" |

### Suspicious Headers

| Purpose | Display Filter |
|---------|---------------|
| Local referer | http.referer contains "127.0.0.1" |
| X-Forwarded-For | http.header contains "X-Forwarded-For" |

### Suspicious Methods

| Purpose | Display Filter |
|---------|---------------|
| Uncommon HTTP methods | http.request.method in {"PUT", "DELETE", "TRACE", "CONNECT"} |

### User Agent Analysis

| Purpose | Display Filter |
|---------|---------------|
| Curl user-agent | http.user_agent contains "curl" |
| Security tool user-agents | http.user_agent matches "(sqlmap\|nikto\|nmap\|acunetix\|fuzz\|dirbuster)" |

Reach out: https://linktr.ee/january1073
