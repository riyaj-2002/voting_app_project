
# 🗳️ Voting App Deployment on EC2 (Ubuntu)

This guide helps you deploy the [Docker Example Voting App](https://github.com/dockersamples/example-voting-app) on an **Amazon EC2 Ubuntu instance** using Docker and Docker Compose.

---

## 🚀 Prerequisites

- EC2 instance running **Ubuntu** (20.04 or later)
- Ports **22**, **80**, and **5000** open in Security Group
- SSH access to your instance

---

## ⚙️ Install Docker and Docker Compose

```bash
# Update and install prerequisites
sudo apt update
sudo apt install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    git

# Add Docker’s GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine and Docker Compose
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Start Docker and enable it at boot
sudo systemctl enable docker
sudo systemctl start docker

# Add your user to the docker group
sudo usermod -aG docker $USER
newgrp docker

# Verify installation
docker --version
docker compose version
