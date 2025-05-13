# Wireshark Display Filters Cheatsheet

## Table of contents
- [Network reconnaissance](#network-reconnaissance)
- [Authentication & credentials](#authentication--credentials)
- [Scanning techniques](#scanning-techniques)
- [DNS analysis](#dns-analysis)
- [Malware & command and control](#malware--command-and-control)
- [Lateral movement](#lateral-movement)
- [Protocol exploits](#protocol-exploits)
- [Data exfiltration](#data-exfiltration)
- [Web application analysis](#web-application-analysis)
- [Suspicious traffic patterns](#suspicious-traffic-patterns)

## Network reconnaissance

### ARP analysis
- ARP spoofing detection: `arp.duplicate-address-detected`
- MAC address mismatch: `arp.src.hw_mac != eth.src`

### LDAP/Active Directory recon
- LDAP bind requests: `ldap.message == 0 or ldap.bindRequest`
- LDAP search requests: `ldap.searchRequest`

### Service discovery
- SSL certificates: `ssl.handshake.certificate`
- SSL on non-standard ports: `ssl.handshake.type == 2 and !(tcp.port == 443)`
- SSL/TLS alerts: `ssl.alert`

## Authentication & credentials

### HTTP authentication
- Basic auth: `http.authorization contains "Basic"`
- NTLM auth: `http.authorization contains "NTLM"`
- WWW authentication headers: `http.www_authenticate`
- Clear-text credentials: `http.request and (http.authorization or http.proxy_authorization)`

### SMB authentication
- NTLM authentication: `ntlmssp.auth.username`
- SMB session setup: `smb.cmd == 0x73`
- SMBv2 session setup: `smb2.cmd == 1`

### SSH authentication
- SSH auth failures: `ssh.message_code == 51`

### SMTP authentication
- SMTP login attempts: `smtp.auth.username`

## Scanning techniques

### Port scanning
- SYN scan: `tcp.flags.syn == 1 and tcp.flags.ack == 0`
- FIN scan: `tcp.flags.fin == 1 and tcp.flags.ack == 0`
- XMAS scan: `tcp.flags == 0x29`

### SSH brute force
- SSH brute force: `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22`
- External SSH brute force: `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22 and ip.src != 192.168.0.0/16`

### Password attacks
- Password spray: `ntlmssp.auth.username and (ntlmssp.ntlmv2.response.len > 0 or ntlmssp.auth.lmresponse.len > 0)`

## DNS analysis

### DNS anomalies
- DNS response errors: `dns.flags.rcode != 0`
- Long DNS query names: `dns.qry.name.len > 50 and dns.qry.type == 16`
- DNS TXT queries: `dns.qry.type == 16`
- Suspicious DNS names: `dns.qry.name matches "[a-z0-9]{20,}"`
- DNS on non-standard port: `dns and not udp.port == 53`
- Large DNS responses: `dns.resp.len > 100`

## Malware & command and control

### Common indicators
- Known malware ports: `tcp.port in {4444, 5554, 6667, 12345, 31337}`
- Specific Metasploit port: `tcp.port == 4444`
- Beaconing behavior: `frame.time_delta < 60 and frame.time_delta > 55`
- Low TTL values: `ip.ttl < 10`

### Tunneling
- ICMP tunneling: `icmp.type == 8 and data.len > 64`

### Anonymization
- Tor traffic: `ip.geoip.country == "A1"`

## Lateral movement

### SMB lateral movement
- SMB IPC$ access: `smb.tree_share == "IPC$"`

### Remote service control
- DCERPC service control: `dcerpc.cn_iface == "367abb81-9844-35f1-ad32-98f038001003"`

## Protocol exploits

### SMB exploits
- SMB Trans2 exploits: `smb.cmd == 37`
- Suspicious SMB errors: `smb.flags.response == 0 and smb.flags.error_code != 0`

### EternalBlue
- EternalBlue exploit: `dcerpc.cn_call_id == 0x1a`

## Data exfiltration

### File transfers
- FTP data transfers: `ftp-data`
- HTTP file uploads: `http.content_type contains "multipart/form-data"`
- HTTP POST requests: `http.request.method == "POST"`

## Web application analysis

### Suspicious headers
- X-Forwarded-For: `http.header contains "X-Forwarded-For"`
- Local referer: `http.referer contains "127.0.0.1"`

### Suspicious methods
- Uncommon HTTP methods: `http.request.method in {"PUT", "DELETE", "TRACE", "CONNECT"}`

### User agent analysis
- Curl user-agent: `http.user_agent contains "curl"`
- Security tool user-agents: `http.user_agent matches "(sqlmap|nikto|nmap|acunetix|fuzz|dirbuster)"`

### Malicious downloads
- Executable downloads: `http.content_type contains "application/x-msdownload"`
- Base64 encoded content: `http.request.uri contains "base64"`

### Common attack indicators
- PHP parameter injection: `http.request.uri contains ".php?"`
- WebShell activity: `http.request.uri contains "cmd=" or http.request.uri contains "exec=" or http.request.uri contains "shell="`

## Suspicious traffic patterns

### DoS attacks
- SYN flood: `tcp.flags.syn == 1 and tcp.flags.ack == 0 and ip.dst == X.X.X.X`
- ICMP flood: `icmp.type == 8 and ip.dst == X.X.X.X`

### Email attachments
- Suspicious SMTP attachments: `smtp.attachment.filename matches "\.(exe|js|scr|vbs|jar)$"`
