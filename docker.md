# Docker Cheatsheet

## Installation

<details>
<summary>Method 1: Simple apt-based installation</summary>

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io
sudo systemctl enable --now docker
```

</details>

<details>
<summary>Method 2: Official Docker repository (recommended)</summary>

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install docker-ce -y
sudo systemctl start docker && sudo systemctl enable docker
```

</details>

---

## Post-Installation Step (Non-root Docker Usage)

```bash
sudo usermod -aG docker $USER
```

> Log out and log back in, or start a new session to apply group membership.

## Container Lifecycle

| Action            | Command                        |
| ----------------- | ------------------------------ |
| List running      | `docker ps`                    |
| List all          | `docker ps -a`                 |
| Start container   | `docker start <container>`     |
| Stop container    | `docker stop <container>`      |
| Restart container | `docker restart <container>`   |
| Remove container  | `docker rm <container>`        |
| Run (new)         | `docker run <options> <image>` |

## Image Management

| Action                | Command                                   |
| --------------------- | ----------------------------------------- |
| List local images     | `docker images`                           |
| Pull from registry    | `docker pull <image>`                     |
| Remove image          | `docker rmi <image>`                      |
| Build from Dockerfile | `docker build -t <name>:<tag> <context>`  |
| Tag image             | `docker tag <image> <new-name>:<new-tag>` |
| Push to registry      | `docker push <name>:<tag>`                |

## Docker Run – Common Options

| Option   | Description                          |
| -------- | ------------------------------------ |
| `-d`     | Detached mode (run in background)    |
| `-p x:y` | Map port y (container) to x (host)   |
| `--name` | Assign name to container             |
| `-v a:b` | Mount volume: host path a → b inside |
| `--rm`   | Remove container after it stops      |
| `-it`    | Interactive with pseudo-TTY          |

Example:

```bash
docker run -d -p 8080:80 --name web nginx
```

## Volume and File Management

| Action                      | Command                        |
| --------------------------- | ------------------------------ |
| Create volume               | `docker volume create <name>`  |
| List volumes                | `docker volume ls`             |
| Inspect volume              | `docker volume inspect <name>` |
| Remove volume               | `docker volume rm <name>`      |
| Bind mount a host directory | `-v /host/dir:/container/dir`  |

## Networks

| Action            | Command                           |
| ----------------- | --------------------------------- |
| List networks     | `docker network ls`               |
| Inspect network   | `docker network inspect <name>`   |
| Create network    | `docker network create <name>`    |
| Remove network    | `docker network rm <name>`        |
| Attach to network | `docker run --network <name> ...` |

## Logs and Diagnostics

| Action             | Command                      |
| ------------------ | ---------------------------- |
| View logs          | `docker logs <container>`    |
| Follow logs live   | `docker logs -f <container>` |
| Top-like stats     | `docker stats`               |
| Inspect container  | `docker inspect <container>` |
| Disk usage summary | `docker system df`           |

## System Cleanup

| Action                    | Command                  |
| ------------------------- | ------------------------ |
| Remove stopped containers | `docker container prune` |
| Remove unused images      | `docker image prune`     |
| Remove unused volumes     | `docker volume prune`    |
| Remove all unused data    | `docker system prune`    |

## Docker Compose (Optional, but Common)

| Action              | Command                     |
| ------------------- | --------------------------- |
| Start services      | `docker-compose up`         |
| Start in background | `docker-compose up -d`      |
| Stop services       | `docker-compose down`       |
| Rebuild services    | `docker-compose up --build` |

Reach out: https://linktr.ee/january1073
