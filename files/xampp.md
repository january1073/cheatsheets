# XAMPP Cheatsheet

## Basic Commands
| Command | Description | Example |
|---------|-------------|---------|
| `sudo /opt/lampp/lampp start` | Start all services (Apache, MySQL, ProFTPD) | |
| `sudo /opt/lampp/lampp stop` | Stop all services | |
| `sudo /opt/lampp/lampp restart` | Restart all services | |
| `sudo /opt/lampp/lampp status` | Check running services | |

## Service-Specific Commands
| Command | Description |
|---------|-------------|
| `sudo /opt/lampp/lampp startapache` | Start Apache only |
| `sudo /opt/lampp/lampp stopapache` | Stop Apache only |
| `sudo /opt/lampp/lampp startmysql` | Start MySQL only |
| `sudo /opt/lampp/lampp stopmysql` | Stop MySQL only |

## MySQL Commands
| Command | Description |
|---------|-------------|
| `sudo /opt/lampp/bin/mysql -u root -p` | Access MySQL shell |
| `sudo /opt/lampp/bin/mysql_secure_installation` | Secure MySQL installation |
| `sudo /opt/lampp/bin/mysqldump -u root -p --all-databases > backup.sql` | Backup all databases |
| `sudo /opt/lampp/bin/mysql -u root -p < backup.sql` | Restore databases |

## Configuration & Logs
| Command | Description |
|---------|-------------|
| `sudo nano /opt/lampp/etc/php.ini` | Edit PHP configuration |
| `sudo nano /opt/lampp/etc/httpd.conf` | Edit Apache configuration |
| `tail -f /opt/lampp/logs/error_log` | View Apache error logs |
| `tail -f /opt/lampp/logs/mysql_error_log` | View MySQL error logs |

## Additional Commands
| Command | Description |
|---------|-------------|
| `sudo /opt/lampp/lampp reload` | Reload services without restart |
| `sudo /opt/lampp/lampp security` | Run XAMPP security check |
| `sudo /opt/lampp/manager-linux-x64.run` | Launch XAMPP GUI (if available) |

### Notes:
- Default installation path: `/opt/lampp/`
- Web root directory: `/opt/lampp/htdocs/`
- Use `-p` flag to be prompted for MySQL password
- Replace `root` with your MySQL username if different

Reach out: https://linktr.ee/january1073
