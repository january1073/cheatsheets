# Wireshark Cheatsheet
![Collection](https://img.shields.io/badge/Collection-DC143C?style=flat-square&logoColor=white) ![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white)

## Installation

### Linux Installation Commands

| Distribution | Command |
|--------------|---------|
| Debian-based | `sudo apt install wireshark` |
| Fedora-based | `sudo dnf install wireshark` |
| Arch-based   | `sudo pacman -S wireshark-qt` |

### Configure Non-Root Packet Capture (Linux)

```bash
sudo groupadd wireshark
sudo usermod -aG wireshark $USER
sudo chgrp wireshark /usr/bin/dumpcap
sudo chmod 750 /usr/bin/dumpcap
sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap
newgrp wireshark
```

Verify with: `getcap /usr/bin/dumpcap` (should return `/usr/bin/dumpcap = cap_net_admin,cap_net_raw=eip`)

### Windows Installation
1. Download installer from https://www.wireshark.org/download.html
2. Install WinPcap/Npcap when prompted
3. Choose whether non-administrators can capture packets

### macOS Installation
- Download .dmg installer from Wireshark website
- Alternatively: `brew install --cask wireshark`

## Interface Overview

| Pane | Description |
|------|-------------|
| Packet List (Top) | Summary information for each packet (number, timestamp, source/destination, protocol, length, info) |
| Packet Details (Middle) | Hierarchical protocol information for selected packet (Ethernet, IP, TCP/UDP, application) |
| Packet Bytes (Bottom) | Raw hexadecimal and ASCII representation of the packet data |

## Default Packet Colors

| Color | Traffic Type |
|-------|-------------|
| Light purple | TCP traffic |
| Light blue | UDP traffic |
| Black | TCP packets with problems |
| Light green | HTTP traffic |
| Dark yellow | Routing protocols |
| Dark green | TCP handshake packets |

*Customize colors via View → Coloring Rules*

## Filtering

### Filter Types

| Filter Type | Applied | Syntax | Purpose |
|-------------|---------|--------|---------|
| Capture Filters | Before packets are captured | BPF syntax (like tcpdump) | Limit what's captured to save resources |
| Display Filters | After packets are captured | Wireshark syntax | Analyze specific aspects of captured traffic |

### Common Capture Filters

| Purpose | Filter |
|---------|--------|
| Traffic to/from specific host | `host 192.168.1.100` |
| Web traffic only | `port 80 or port 443` |
| DNS traffic | `port 53` |
| Exclude broadcast/multicast | `not broadcast and not multicast` |

### Common Display Filters

| Purpose | Filter |
|---------|--------|
| Filter by IP address | `ip.addr == 192.168.1.100` |
| Filter by protocol | `http` or `dns` or `ssh` |
| Filter by port | `tcp.port == 443` |
| Filter by HTTP method | `http.request.method == "POST"` |
| Filter by content | `http.request.uri contains "login"` |

### Combining Display Filters

| Operator | Symbols | Example |
|----------|---------|---------|
| AND | `and` or `&&` | `http.request.method == "POST" and ip.addr == 192.168.1.100` |
| OR | `or` or `\|\|` | `tcp.port == 80 or tcp.port == 443` |
| NOT | `not` or `!` | `not arp` |

### Security-Focused Filters

| Purpose | Filter |
|---------|--------|
| Potential SSH brute force | `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 22` |
| SMB password attempts | `ntlmssp.auth.username` |
| HTTP Basic Auth | `http.authorization contains "Basic"` |
| Certificate issues | `ssl.alert` |
| Non-standard DNS traffic | `dns and not udp.port == 53` |

## Protocol Analysis

### HTTP Analysis

| Purpose | Filter |
|---------|--------|
| All HTTP traffic | `http` |
| HTTP POST requests | `http.request.method == "POST"` |
| Extract form data | Find HTTP POST packets and examine form fields in packet details |

### DNS Analysis

| Purpose | Filter |
|---------|--------|
| DNS queries only | `dns.flags.response == 0` |
| DNS responses only | `dns.flags.response == 1` |
| Potential DNS tunneling | `dns.qry.name.len > 50` |

## Decrypting TLS Traffic

### SSLKEYLOGFILE Method Setup

| OS | Environment Variable | Browser Launch |
|----|---------------------|----------------|
| Linux (Firefox) | `export SSLKEYLOGFILE=~/.ssl-key.log` | Launch Firefox normally |
| Linux (Chrome) | `export SSLKEYLOGFILE=~/.ssl-key.log` | `google-chrome --ssl-key-log-file=$SSLKEYLOGFILE` |
| macOS (Firefox) | `export SSLKEYLOGFILE=~/.ssl-key.log` | Launch Firefox normally |
| macOS (Chrome) | `export SSLKEYLOGFILE=~/.ssl-key.log` | `open /Applications/Google\ Chrome.app --args --ssl-key-log-file=$SSLKEYLOGFILE` |
| Windows (Firefox) | PowerShell: `$env:SSLKEYLOGFILE="$HOME\ssl-key.log"` | Launch Firefox normally |
| Windows (Chrome) | PowerShell: `$env:SSLKEYLOGFILE="$HOME\ssl-key.log"` | `Start-Process "chrome.exe" -ArgumentList "--ssl-key-log-file=$env:SSLKEYLOGFILE"` |

### Configure Wireshark for TLS Decryption
1. Go to Edit → Preferences → Protocols → TLS
2. Set "(Pre)-Master-Secret log filename" to your keylog file path

## Extracting Sensitive Information

### Authentication Protocol Filters

| Authentication Protocol | Wireshark Display Filter Expression |
|-------------------------|-------------------------------------|
| FTP | `ftp.request.command == "USER" or ftp.request.command == "PASS"` |
| HTTP Basic Auth | `http.authorization contains "Basic"` |
| HTTP Form-based Auth | `http.request.method == "POST" and http.request.uri contains "login"` |
| IMAP | `imap.request.command == "LOGIN"` |
| Kerberos | `kerberos` |
| LDAP | `ldap` |
| MySQL | `mysql` |
| NTLM | `ntlmssp` |
| POP3 | `pop.request.command == "USER" or pop.request.command == "PASS"` |
| RDP (Remote Desktop) | `rdp` |
| SIP (VoIP) | `sip.Authorization` |
| SMB (Windows File Share) | `smb2 or smb` |
| SNMP | `snmp` |
| SMTP | `smtp.auth` |
| Telnet | `telnet` |

### Extracting Files
1. **HTTP**:
   - File → Export Objects → HTTP
   - Select files to save

2. **General Method**:
   - Right-click packet → Follow → [Protocol] Stream
   - In new window, click "Save As"

### Session Analysis

| Purpose | Filter |
|---------|--------|
| Find cookies | `http.cookie or http.set_cookie` |
| Follow user activity | `ip.addr == [user_ip]` |
| Wi-Fi handshakes | `eapol` |

## Statistics and Analysis Tools

| Tool | Menu Location | Purpose |
|------|---------------|---------|
| Protocol Hierarchy | Statistics → Protocol Hierarchy | View breakdown of protocols by percentage |
| Conversations | Statistics → Conversations | See connection summaries |
| Endpoints | Statistics → Endpoints | List all active hosts |
| I/O Graph | Statistics → I/O Graph | Visualize traffic patterns over time |
| Flow Graph | Statistics → Flow Graph | See packet exchange sequences |

## Tips for Security Analysis
- Establish baseline network behavior before looking for anomalies
- Save complex filters for reuse
- Use color rules to highlight suspicious traffic
- Combine protocol analysis with endpoint behavior
- Look for unusual patterns (high volumes, odd ports, unexpected protocols)
- More display filter expressions here: [Wireshark Display Filters (Examples)]

Reach out: https://linktr.ee/january1073
