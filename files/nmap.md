# Nmap Cheatsheet
![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white) ![Execution](https://img.shields.io/badge/Execution-DC143C?style=flat-square&logoColor=white) ![Initial Access](https://img.shields.io/badge/Initial%20Access-DC143C?style=flat-square&logoColor=white) ![Reconnaissance](https://img.shields.io/badge/Reconnaissance-DC143C?style=flat-square&logoColor=white)

## Target Specification

| Command | Description |
|---------|-------------|
| nmap 192.168.1.100 | Single IP address |
| nmap 192.168.1.1-100 | IP range |
| nmap 192.168.1.0/24 | CIDR notation |
| nmap example.com | Host name |
| nmap 192.168.1.1 192.168.1.5 example.com | Multiple targets |
| nmap -iL targets.txt | Input file |
| nmap 192.168.1.0/24 --exclude 192.168.1.1 | Exclude targets |

## DNS Control

| Command | Description |
|---------|-------------|
| nmap -n &lt;target&gt; | Skip DNS resolution |
| nmap -R &lt;target&gt; | Force DNS resolution |
| nmap --dns-servers 8.8.8.8,1.1.1.1 &lt;target&gt; | Use custom DNS servers |

## Host Discovery

| Command | Description |
|---------|-------------|
| nmap -sn &lt;target&gt; | Ping sweep (no port scan) |
| nmap -PE &lt;target&gt; | ICMP Echo ping |
| nmap -PP &lt;target&gt; | ICMP Timestamp ping |
| nmap -PM &lt;target&gt; | ICMP Address Mask ping |
| nmap -PS80,443 &lt;target&gt; | TCP SYN ping to specific ports |
| nmap -PA80,443 &lt;target&gt; | TCP ACK ping to specific ports |
| nmap -PU53,161 &lt;target&gt; | UDP ping to specific ports |
| nmap -PR 192.168.1.0/24 | ARP discovery (local network) |
| nmap -Pn &lt;target&gt; | Skip ping (assume host is up) |

### Corporate Network Discovery
```bash
# TCP SYN ping to common ports
nmap -PS22,80,443 192.168.1.0/24

# TCP ACK ping (useful against stateful firewalls)
nmap -PA80 192.168.1.0/24

# UDP ping to common services
nmap -PU53,161 192.168.1.0/24
```

### Local Network Comprehensive Discovery
```bash
nmap -PE -PP -PM -PS21,22,23,25,53,80,110,111,135,139,143,443,993,995 -PA80,113,443,10042 -PU40125 192.168.1.0/24
```

## Port Specification

| Command | Description |
|---------|-------------|
| nmap -p 22,80,443 &lt;target&gt; | Specific ports |
| nmap -p 1-1000 &lt;target&gt; | Port range |
| nmap &lt;target&gt; | Top 1000 most common ports (default) |
| nmap --top-ports 100 &lt;target&gt; | Top n ports |
| nmap -p- &lt;target&gt; | All 65535 ports |
| nmap -p 1-1000 --exclude-ports 135,139 &lt;target&gt; | Exclude specific ports |

## Port Scanning Types

| Scan Type | Command | Description | Privileges Required |
|-----------|---------|-------------|-------------------|
| TCP Connect | nmap -sT &lt;target&gt; | Full TCP handshake | User |
| TCP SYN (Stealth) | nmap -sS &lt;target&gt; | Half-open scan | Root/Admin |
| UDP | nmap -sU &lt;target&gt; | UDP port scan | Root/Admin |
| TCP Window | nmap -sW &lt;target&gt; | Window scan | Root/Admin |
| Null | nmap -sN &lt;target&gt; | No flags set | Root/Admin |
| FIN | nmap -sF &lt;target&gt; | FIN flag only | Root/Admin |
| Xmas | nmap -sX &lt;target&gt; | FIN, PSH, URG flags | Root/Admin |

## Timing Templates

| Flag | Name | Description |
|------|------|-------------|
| -T0 | Paranoid | Very slow, avoid IDS |
| -T1 | Sneaky | Slow, avoid IDS |
| -T2 | Polite | Slow, less bandwidth |
| -T3 | Normal | Default timing |
| -T4 | Aggressive | Fast, reliable networks |
| -T5 | Insane | Very fast, may miss results |

**Example:** nmap -T4 192.168.1.0/24

## Service and OS Detection

| Command | Description |
|---------|-------------|
| nmap -sV &lt;target&gt; | Service version detection |
| nmap -sV --version-intensity 5 &lt;target&gt; | Set intensity level (0-9) |
| nmap -sV --version-all &lt;target&gt; | All probes for maximum accuracy |
| nmap -O &lt;target&gt; | OS detection |
| nmap -O --osscan-guess &lt;target&gt; | Aggressive OS detection |
| nmap -O --max-os-tries 2 &lt;target&gt; | Limit OS scan attempts |
| nmap -A &lt;target&gt; | Aggressive scan (OS, version, scripts, traceroute) |

## Nmap Scripting Engine (NSE)

### Basic Script Usage

| Command | Description |
|---------|-------------|
| nmap -sC &lt;target&gt; | Run default scripts |
| nmap --script=vuln &lt;target&gt; | Run all vulnerability scripts |
| nmap --script=http-enum,http-headers &lt;target&gt; | Run specific scripts |
| nmap --script="http-*" &lt;target&gt; | Run scripts with wildcards |

### Script Categories

| Category | Description |
|----------|-------------|
| auth | Authentication testing |
| broadcast | Network broadcast discovery |
| brute | Brute force attacks |
| default | Scripts run with -sC |
| discovery | Host and service discovery |
| dos | Denial of service testing |
| exploit | Exploitation attempts |
| external | Scripts requiring external resources |
| fuzzer | Fuzzing tests |
| intrusive | Aggressive tests that may crash services |
| malware | Malware detection |
| safe | Safe scripts unlikely to crash services |
| version | Version detection enhancement |
| vuln | Vulnerability detection |

### Common Script Examples

| Purpose | Command |
|---------|---------|
| Web application testing | nmap --script=http-enum &lt;target&gt; |
| Check web vulnerabilities | nmap --script=http-vuln-* &lt;target&gt; |
| HTTP methods enumeration | nmap --script=http-methods &lt;target&gt; |
| Web application firewall detection | nmap --script=http-waf-detect &lt;target&gt; |
| MySQL information | nmap --script=mysql-info &lt;target&gt; |
| Database brute force | nmap --script=mysql-brute &lt;target&gt; |
| SMB OS discovery | nmap --script=smb-os-discovery &lt;target&gt; |
| SMB vulnerability scanning | nmap --script=smb-vuln-* &lt;target&gt; |
| SMB shares enumeration | nmap --script=smb-enum-shares &lt;target&gt; |
| Banner grabbing | nmap --script=banner &lt;target&gt; |

### Script Arguments

| Command | Description |
|---------|-------------|
| nmap --script=http-brute --script-args http-brute.path=/admin &lt;target&gt; | HTTP brute force with custom path |
| nmap --script=dns-brute --script-args dns-brute.hostlist=hosts.txt &lt;target&gt; | DNS brute force with custom list |
| nmap --script=snmp-brute --script-args snmp-brute.communitiesdb=communities.txt &lt;target&gt; | SNMP brute force with custom strings |

### Script Performance

| Command | Description |
|---------|-------------|
| nmap --script=vuln --script-args timeout=30s &lt;target&gt; | Control script timeout |
| nmap --script=brute --script-args threads=5 &lt;target&gt; | Limit script threads |
| nmap --script=http-enum --script-trace &lt;target&gt; | Debug script execution |

## Output Formats

| Command | Description |
|---------|-------------|
| nmap &lt;target&gt; | Normal output (default) |
| nmap -v &lt;target&gt; | Verbose output |
| nmap -oN scan_results.txt &lt;target&gt; | Save normal output to file |
| nmap -oX scan_results.xml &lt;target&gt; | XML output for parsing |
| nmap -oG scan_results.gnmap &lt;target&gt; | Grepable output |
| nmap -oA scan_results &lt;target&gt; | All formats simultaneously |

## Optimization Strategies

### Fast Scanning (Large Networks)

| Command | Description |
|---------|-------------|
| nmap -T4 --top-ports 100 192.168.1.0/24 | Fast scan of top 100 ports |
| nmap -Pn -p 22,80,443 -T4 192.168.1.0/24 | Skip discovery, scan specific ports |

### Comprehensive Scanning (Specific Targets)

| Command | Description |
|---------|-------------|
| nmap -p- -T4 &lt;target&gt; | All TCP ports |
| nmap -sSU -T4 &lt;target&gt; | Combined TCP and UDP scan |
| nmap -p- -sV -T4 &lt;target&gt; | All ports with service detection |

### UDP Optimization

| Command | Description |
|---------|-------------|
| nmap -sU --top-ports 100 &lt;target&gt; | Scan common UDP ports only |
| nmap -sU -sV &lt;target&gt; | UDP scan with version detection |
| nmap -sU -T4 &lt;target&gt; | Faster UDP scanning |

## Advanced Scanning Techniques

### Idle (Zombie) Scan

| Command | Description |
|---------|-------------|
| nmap -O -v &lt;zombie_candidate&gt; | Identify potential zombie hosts |
| nmap -sI &lt;zombie_host&gt;:&lt;port&gt; &lt;target&gt; | Perform idle scan |

### FTP Bounce Scan

| Command | Description |
|---------|-------------|
| nmap -b &lt;ftp_server&gt; &lt;target&gt; | FTP bounce scan (legacy) |

### Fragmentation and Evasion

| Command | Description |
|---------|-------------|
| nmap -f &lt;target&gt; | Fragment packets (8-byte pieces) |
| nmap -ff &lt;target&gt; | Fragment packets (16-byte pieces) |
| nmap --mtu 24 &lt;target&gt; | Custom fragment size |
| nmap -D &lt;decoy1&gt;,&lt;decoy2&gt;,ME &lt;target&gt; | Decoy scan with specific IPs |
| nmap -D RND:10 &lt;target&gt; | Random decoy scan |
| nmap --source-port 53 &lt;target&gt; | Use specific source port |
| nmap -g 20 &lt;target&gt; | Use source port 20 (FTP-DATA) |

## Common Command Combinations

### Basic Network Survey
```bash
# Discover live hosts
nmap -sn 192.168.1.0/24

# Fast port scan
nmap -sS -T4 --top-ports 1000 192.168.1.0/24
```

### Comprehensive Target Analysis
```bash
# Aggressive scan
nmap -A -T4 <target>

# Custom aggressive combination
nmap -sS -sV -O -sC -T4 <target>
```

### Stealth Reconnaissance
```bash
# Stealth scan with fragmentation and decoys
nmap -sS -T2 -f -D RND:10 <target>
```

### Service-Specific Assessments
```bash
# Web application assessment
nmap -p 80,443 -sV --script=http-* <target>

# Database security testing
nmap -p 3306 --script=mysql-* <target>

# SMB/NetBIOS enumeration
nmap -p 139,445 --script=smb-* <target>
```

Reach out: https://guns.lol/january1073
