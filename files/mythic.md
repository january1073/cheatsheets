# Mythic C2 Framework Cheatsheet
![Collection](https://img.shields.io/badge/Collection-DC143C?style=flat-square&logoColor=white) ![Command and Control](https://img.shields.io/badge/Command%20and%20Control-DC143C?style=flat-square&logoColor=white) ![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Defense Evasion](https://img.shields.io/badge/Defense%20Evasion-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white) ![Execution](https://img.shields.io/badge/Execution-DC143C?style=flat-square&logoColor=white) ![Exfiltration](https://img.shields.io/badge/Exfiltration-DC143C?style=flat-square&logoColor=white) ![Impact](https://img.shields.io/badge/Impact-DC143C?style=flat-square&logoColor=white) ![Initial Access](https://img.shields.io/badge/Initial%20Access-DC143C?style=flat-square&logoColor=white) ![Lateral Movement](https://img.shields.io/badge/Lateral%20Movement-DC143C?style=flat-square&logoColor=white) ![Persistence](https://img.shields.io/badge/Persistence-DC143C?style=flat-square&logoColor=white) ![Privilege Escalation](https://img.shields.io/badge/Privilege%20Escalation-DC143C?style=flat-square&logoColor=white) ![Reconnaissance](https://img.shields.io/badge/Reconnaissance-DC143C?style=flat-square&logoColor=white) ![Resource Development](https://img.shields.io/badge/Resource%20Development-DC143C?style=flat-square&logoColor=white)

## Basic Server Management
| Command | Description |
|---------|-------------|
| `~/Mythic/mythic-cli start` | Start the Mythic server |
| `~/Mythic/mythic-cli stop` | Stop the Mythic server |
| `~/Mythic/mythic-cli restart` | Restart the Mythic server |
| `~/Mythic/mythic-cli status` | Check server status |
| `~/Mythic/mythic-cli logs` | View server logs |

## Container Management
| Command | Description |
|---------|-------------|
| `docker compose -f ~/Mythic/docker-compose.yml ps` | List Mythic containers |
| `docker compose -f ~/Mythic/docker-compose.yml logs` | View all container logs |
| `docker compose -f ~/Mythic/docker-compose.yml logs -f <service>` | Follow logs for specific service |
| `docker exec -it <container_name> bash` | Enter a running container |

## Installation & Updates
| Command | Description |
|---------|-------------|
| `git clone https://github.com/its-a-feature/Mythic ~/Mythic` | Clone Mythic repository |
| `cd ~/Mythic && ./install_docker_ubuntu.sh` | Install Docker (Ubuntu) |
| `cd ~/Mythic && ./mythic-cli install` | Install Mythic components |
| `cd ~/Mythic && git pull` | Update Mythic code |
| `~/Mythic/mythic-cli update` | Update containers |

## Agent & Payload Management
| Command | Description |
|---------|-------------|
| `~/Mythic/mythic-cli payload add <path>` | Add a new payload |
| `~/Mythic/mythic-cli payload list` | List available payloads |
| `~/Mythic/mythic-cli agent list` | List installed agents |
| `~/Mythic/mythic-cli agent install <name>` | Install an agent |

## Configuration
| Command | Description |
|---------|-------------|
| `~/Mythic/mythic-cli config get` | View current config |
| `~/Mythic/mythic-cli config set <key> <value>` | Change config value |
| `~/Mythic/mythic-cli reset` | Reset Mythic database |

## Troubleshooting
| Command | Description |
|---------|-------------|
| `docker system prune -a` | Clean up Docker system (WARNING: removes unused containers/images) |
| `sudo service docker restart` | Restart Docker service |
| `journalctl -u docker --no-pager -n 50` | View Docker service logs |


Reach out: https://linktr.ee/january1073
