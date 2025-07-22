
# rl-swarm Setup & Usage Guide

Welcome to the **rl-swarm** project! This guide covers how to set up the project, replace your old swarm key, run the node safely with screen or as a background process, and troubleshoot common issues.

---

## Table of Contents

- [Prerequisites](#prerequisites)  
- [Installation](#installation)  
- [Replacing the Old Swarm Key](#replacing-the-old-swarm-key)  
- [Running the Node](#running-the-node)  
- [Using Screen to Run Node in Background](#using-screen-to-run-node-in-background)  
- [Checking Logs](#checking-logs)  
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

- Ubuntu 20.04+ or compatible Linux distribution  
- Docker & Docker Compose installed ([Official Docker install guide](https://docs.docker.com/engine/install/ubuntu/))  
- Basic Linux command-line knowledge  
- Your old `swarm.pem` key file ready to be added

---

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/rl-swarm.git
cd rl-swarm
```

2. Install Docker & Compose (if not installed):

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
```

---

## Replacing the Old Swarm Key

Ensure the keys directory exists and you own it:

```bash
sudo chown -R yourvpsusername:yourvpsusername ./user/keys
```

Delete the existing swarm.pem key (if it exists):

Upload your old swarm.pem file to the ./user/keys/ directory:

Or upload using your favorite SFTP client.


---

## Running the Node

Start the node with Docker Compose:

```bash
screen -S gensyn
```

```bash
python3 -m venv .venv
source .venv/bin/activate
docker compose run --rm --build --service-ports swarm-cpu
```

If you face permission errors, make sure your user is added to the Docker group:

```bash
sudo usermod -aG docker $USER
newgrp docker  # or log out and back in
```
now restart your vps again:

```bash
docker compose run --rm --build --service-ports swarm-cpu
```

## open same vps with new terminal 

```bash
npx localtunnel --port 3000
```

When web ask for pass then give your ip in the password box 
Verify your email done enjoy 

---

Detach from the screen session by pressing:

```
Ctrl + A, then D
```

You can now safely close your SSH client.

To reconnect to the session:

```bash
screen -r gensyn
```

---

## Troubleshooting

**Permission denied connecting to Docker daemon:**  
Add user to docker group and re-login:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

**Cannot delete or overwrite swarm.pem:**  
Check ownership and permissions:

```bash
sudo chown -R yourvpsusername:yourvpsusername ./user/keys
```

File might be immutable (rare):  
Remove immutable flag:

```bash
sudo chattr -i ./user/keys/swarm.pem
```

---
