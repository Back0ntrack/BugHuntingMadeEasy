# Docker Fundamentals

## Docker

**Docker** is an open-source containerization platform that packages applications and their dependencies into lightweight, portable, isolated containers using OS-level virtualization, enabling consistent development, testing, deployment, scalability, and efficient resource utilization across different environments.

### Installation & Startup

```zsh
# Update the repositories
sudo apt update
sudo apt install -y docker.io docker-compose-plugin

# Restart the service on reboot
sudo systemctl enable docker

# Start the service 
sudo systemctl start docker
```

### Basic Usage

```zsh
# Check if installation is successfull
docker --version

# List images
docker images

# Search for an image 
docker search nginx

# Pull images
docker pull robensive/assetfinder

# Remove an image 
docker rmi robensive/assetfinder -f
```

### Inspection and debugging

```zsh
# Inspect docker at runtime
docker inspect <container>
docker inspect <image>

# Runtime checking of the docker images
docker run --rm -it --entrypoint sh <image_name>
```

### Execution and Stop

```zsh
# Execute a docker image 
docker run back0ntrack/subfinder

# Find id of the docker for killing
docker ps

# Kill the docker (Use any one command)
docker stop <id>
docker kill <id>
```

### Normal User in Docker

```bash
sudo usermod -aG docker r0b
```

## Container

A Docker image is a fixed environment that never changes. When we run an image like `robensive/assetfinder`, Docker creates a new container (usually based on Alpine) with a random name. Task-Ninja runs the tool inside this container, collects the output in `output.txt`, and then the container stops. Even after stopping, the container still uses disk space, so we need to remove this unused data using Docker prune commands.

```zsh
# List all docker containers (either running or exited)
docker ps -a

# Remove unused space
docker container prune -f
```

## Why being in the `docker` group is powerful

#### What “docker group” really means

If your user is in the `docker` group:

* You can talk directly to: `/var/run/docker.sock`<br>
* That socket controls the **Docker daemon**<br>
* The Docker daemon runs as **root**<br>

So effectively:

`You → Docker socket → Root daemon → Root actions`

No sudo required.

A user who is only in the Docker group can indirectly gain root-level control of the system because Docker commands are executed by a root-running daemon. By running containers that mount host filesystems or expose network ports, such a user can modify system files (including passwords) or run bind/reverse shells without needing sudo. Therefore, Docker access must be treated as equivalent to root, and untrusted images or workflows should never be executed.

## Power of docker

There's a high risk of indirect remote access via reverse shell if the containers are maliciously designed (e.g., with persistent backdoors in the binaries or entrypoints). By mounting host filesystems (e.g., via -v /:/host) and running non-exiting processes that scan/execute host commands, the container could exfiltrate data or bind to ports for callbacks. Anonymous naming (e.g., "handy-linux-tools" or "win-sys-utils") lures pulls from public registries like Docker Hub, exploiting curiosity or typosquatting.

### Attack flow diagram

```bash
Victim Pulls Image → Runs Container (--privileged or vol mounts)
          ↓
Container Accesses Host FS → Executes Malicious Binary (e.g., nc/wget for callback)
          ↓
Binary Establishes Reverse Shell → Attacker Receives Connection (e.g., netcat listener)
          ↓
Remote Access Achieved (RCE on host via container escape)
```

## Advanced Docker Configuration for Remote Access using Docker

### Mounting host files/directories into a container

#### 🔹 Bind-mount a host directory

```bash
docker run -v /host/path:/container/path image
```

**What happens**

```bash
Host directory  →  Docker mount  →  Container directory
```

Example:

```bash
docker run -v /home/r0b/output:/data image
```

* Anything written to `/data` in the container appears in `/home/r0b/output` on the host and vice versa.

```bash
Container writes file → /data/output.txt
            │
            ▼
Docker bind mount
            │
            ▼
Host file appears → /home/r0b/results/output.txt
```

### Exposing container ports to the host

#### 🔹 Publish a container port to a host port

```bash
docker run -p <host_port>:<container_port> image
```

Example:

```bash
docker run -p 4444:4444 image
```

**Traffic flow**

```bash
Host:4444  →  Docker NAT  →  Container:4444
```

**Expose all declared ports automatically**

```bash
docker run -P image
```
