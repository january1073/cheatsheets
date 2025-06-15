# SSH & SCP Cheatsheet

## Basic SSH Usage

| Action                     | Command                          |
| -------------------------- | -------------------------------- |
| Connect to remote host     | `ssh user@host`                  |
| Specify port               | `ssh -p 2222 user@host`          |
| Use identity (private key) | `ssh -i ~/.ssh/id_rsa user@host` |
| Run remote command         | `ssh user@host "command"`        |
| Exit session               | `exit`                           |

## SSH Key Management

| Action                             | Command                                                 |
| ---------------------------------- | ------------------------------------------------------- |
| Generate key pair (RSA, 4096 bits) | `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` |
| Generate Ed25519 key               | `ssh-keygen -t ed25519 -C "your_email@example.com"`     |
| Specify custom key filename        | `ssh-keygen -f ~/.ssh/custom_key`                       |
| Copy public key to remote host     | `ssh-copy-id user@host`                                 |
| Manually append public key         | Append to `~/.ssh/authorized_keys` on remote            |
| List loaded keys                   | `ssh-add -l`                                            |
| Add key to ssh-agent               | `ssh-add ~/.ssh/id_rsa`                                 |
| Start ssh-agent                    | `eval "$(ssh-agent -s)"`                                |

## SSH Config File (`~/.ssh/config`)

Example:

```ssh
Host myserver
    HostName example.com
    User username
    Port 2222
    IdentityFile ~/.ssh/id_rsa
```

Use with:

```bash
ssh myserver
```

| Purpose            | Field          |
| ------------------ | -------------- |
| Alias for the host | `Host`         |
| Real address       | `HostName`     |
| SSH login name     | `User`         |
| Custom port        | `Port`         |
| Use specific key   | `IdentityFile` |

## SCP – Secure Copy

| Action                     | Command                                        |
| -------------------------- | ---------------------------------------------- |
| Copy local → remote        | `scp file.txt user@host:/remote/path/`         |
| Copy remote → local        | `scp user@host:/remote/file.txt ./`            |
| Copy directory (recursive) | `scp -r dir/ user@host:/remote/path/`          |
| Specify port               | `scp -P 2222 file.txt user@host:/remote/path/` |
| Use custom identity file   | `scp -i ~/.ssh/id_rsa file user@host:/path/`   |

## SSH Port Forwarding

| Type                               | Command Example                      |
| ---------------------------------- | ------------------------------------ |
| Local (localhost:8080 → remote:80) | `ssh -L 8080:localhost:80 user@host` |
| Remote (host:8080 → local:80)      | `ssh -R 8080:localhost:80 user@host` |
| Dynamic (SOCKS proxy on localhost) | `ssh -D 1080 user@host`              |

## Multiplexing (Optional Performance Optimization)

Enable persistent SSH connections via `~/.ssh/config`:

```ssh
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h:%p
    ControlPersist 10m
```

> Create the socket directory first:

```bash
mkdir -p ~/.ssh/sockets
```

## Security Hardening (for Server Admins)

| Task                      | File / Setting                |
| ------------------------- | ----------------------------- |
| Disable root login        | `PermitRootLogin no`          |
| Allow only specific users | `AllowUsers alice bob`        |
| Disable password login    | `PasswordAuthentication no`   |
| Disable empty passwords   | `PermitEmptyPasswords no`     |
| Change SSH port           | `Port 2222`                   |
| Configuration file        | `/etc/ssh/sshd_config`        |
| Restart SSH daemon        | `sudo systemctl restart sshd` |

## Troubleshooting & Debugging

| Action                         | Command                                                  |
| ------------------------------ | -------------------------------------------------------- |
| Verbose SSH output             | `ssh -v user@host`                                       |
| Test SSH port                  | `nc -vz host 22` or `telnet host 22`                     |
| Check SSH service status       | `sudo systemctl status sshd`                             |
| View auth logs (Debian/Ubuntu) | `sudo journalctl -u ssh` or `sudo cat /var/log/auth.log` |

Reach out: https://linktr.ee/january1073
