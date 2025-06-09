# Linux System Reconnaissance Cheatsheet

## System Information

| Command               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `hostname`            | Display or set the system's hostname                                        |
| `uname -a`            | Show kernel version and system information                                  |
| `cat /proc/version`   | Display kernel version and compiler information                             |
| `cat /etc/issue`      | Show operating system information                                           |
| `cat /etc/os-release` | Display detailed OS information (modern systems)                            |

## Process Management

| Command          | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `ps -A`          | List all running processes                                                  |
| `ps axjf`        | Display process tree                                                        |
| `ps aux`         | Show all processes for all users with detailed information                 |
| `top` / `htop`   | Interactive process viewer                                                  |
| `kill -l`        | List available signals for process management                               |

## User and Environment

| Command                     | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| `id`                        | Show user and group information                                             |
| `whoami`                    | Display current username                                                    |
| `env`                       | List environment variables (check PATH for useful tools)                    |
| `sudo -l`                   | List commands the current user can run with sudo                            |
| `cat /etc/passwd`           | List all system users                                                       |
| `cat /etc/passwd \| cut -d ":" -f 1` | Extract usernames only                                       |
| `cat /etc/passwd \| grep home` | Filter for real users with home directories                                |
| `history`                   | Show command history (may contain sensitive information)                    |

## Network Information

| Command               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `ifconfig` / `ip a`   | Display network interfaces and configurations                              |
| `ip route`            | Show routing table                                                         |
| `netstat -a`          | List all listening ports and established connections                       |
| `netstat -at`         | Show TCP connections                                                       |
| `netstat -au`         | Show UDP connections                                                       |
| `netstat -l`          | Display listening ports                                                    |
| `netstat -s`          | Show network statistics by protocol                                        |
| `netstat -tp`         | List connections with service names and PIDs                               |
| `netstat -ltp`        | Show listening ports with service names and PIDs                           |
| `netstat -i`          | Display interface statistics                                               |
| `netstat -ano`        | Show all sockets without name resolution, with timers                      |

## File System Exploration

| Command | Description |
|---------|-------------|
| `ls -la` | List directory contents with permissions and hidden files |
| `find . -name flag1.txt` | Find file named "flag1.txt" in current directory |
| `find /home -name flag1.txt` | Find file named "flag1.txt" in /home directory |
| `find / -type d -name config` | Find directory named "config" under root |
| `find / -type f -perm 0777` | Find files with 777 permissions |
| `find / -perm a=x` | Find executable files |
| `find /home -user frank` | Find files owned by user "frank" under /home |
| `find / -mtime 10` | Find files modified in last 10 days |
| `find / -atime 10` | Find files accessed in last 10 days |
| `find / -cmin -60` | Find files changed in last hour |
| `find / -amin -60` | Find files accessed in last hour |
| `find / -size 50M` | Find files of exactly 50MB |
| `find / -writable -type d 2>/dev/null` | Find world-writable directories |
| `find / -perm -222 -type d 2>/dev/null` | Find directories writable by user, group, others |
| `find / -perm -o=w -type d 2>/dev/null` | Find directories others can write to |
| `find / -perm -o x -type d 2>/dev/null` | Find world-executable folders |
| `find / -name perl*` | Find Perl-related files |
| `find / -name python*` | Find Python-related files |
| `find / -name gcc*` | Find GCC compiler files |
| `find / -perm -u=s -type f 2>/dev/null` | Find SUID files (potential privilege escalation) |

## Useful Tools

| Command | Description |
|---------|-------------|
| `grep`  | Powerful text search |
| `cut`   | Extract sections from lines |
| `sort`  | Sort lines of text |
| `locate` | Find files quickly (uses database) |
| `awk`   | Text processing language |
| `sed`   | Stream editor |
| `nc` / `netcat` | Networking Swiss army knife |
| `curl` / `wget` | Download files from the web |

## Tips
- Redirect errors with `2>/dev/null` to clean up output
- Use `man <command>` for detailed command information
- Combine commands with pipes (`|`) for powerful workflows
- Check `/tmp` and `/dev/shm` for temporary files
- Look for hidden files and directories (names starting with `.`)
```
Reach out: https://linktr.ee/january1073
