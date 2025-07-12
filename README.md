
# 🗳️ Voting App Deployment 

This project is a full-stack voting application with:

- **Frontend:** Python (e.g., Flask) and Node.js (Express)
- **Backend API:** .NET (Worker) 
- **Database:** Redis for messaging and Postgres for storage.


Deployed via Docker Compose on an **Ubuntu EC2** instance.

---

## 🚀 Prerequisites

- EC2 instance running **Ubuntu** 
- Ports **22**, **80**, and **5000** open in Security Group
- SSH access to your instance

---

## 🛠️ AWS Setup: VPC, Subnets, Route Table, Internet Gateway and EC2 Instance

### 1. Create a VPC

   - **VPC Name:** `vpc-project`
   - **IPv4 CIDR block:** `10.0.0.0/24`
   - Set **Tenancy** as `Default`.
     

  ![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20152216.png)   


## 🛠️ EC2 Instance Setup

### 1. Launch EC2 Instance

   - **InstanceType:** t2.medium
   - **KeyName:** key.pair.pem
   - **VPC:** vpc-project
   - **SUBNET:** pub-sub-project
   - **SECURITY GROUP:** SSH (port 22) , HTTP (port 80) , HTTPS (port 443) 

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-04-29%20143622.png)


## ⚙️ Install Docker and Docker Compose

```bash
#!/bin/bash
# Setup Docker Engine and Docker Compose (v2)

set -euo pipefail

# Update and install Docker
sudo apt update -y
sudo apt install -y docker.io curl

# Enable and start Docker service
sudo systemctl enable --now docker

# Install Docker Compose v2 (standalone)
COMPOSE_VERSION="v2.38.2"
DESTINATION="/usr/local/bin/docker-compose"

sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o "$DESTINATION"
sudo chmod +x "$DESTINATION"

# Optional: create symlink for backward compatibility
if ! command -v docker-compose &> /dev/null; then
    sudo ln -s "$DESTINATION" /usr/bin/docker-compose
fi

# Print versions to verify
docker --version
docker-compose --version


```

---

## Clone the Voting App Repository

```bash
git clone https://github.com/riyaj-2002/voting_app_roject.git
cd voting_app_project
```

---
## Push the Docker Images to DockerHub

### Vote Service
```bash
docker build --target final -t 17082002/voting-app:vote ./vote
docker push 17082002/voting-app:vote
```

### Result Service
```bash
docker build -t 17082002/voting-app:result ./result
docker push 17082002/voting-app:result
```

### Worker Service
```bash
docker build -t 17082002/voting-app:worker ./worker
docker push 17082002/voting-app:worker
```
---

## Deploy All Services to Kubernetes

### Apply all deployments
```bash
kubectl apply -f redis-deployment.yaml
kubectl apply -f postgres-deployment.yaml
kubectl apply -f vote-deployment.yaml
kubectl apply -f result-deployment.yaml
kubectl apply -f worker-deployment.yaml
```

### Apply All services
```bash
kubectl apply -f redis-service.yaml
kubectl apply -f postgres-service.yaml
kubectl apply -f vote-service.yaml
kubectl apply -f result-service.yaml
```

## ✅ Application is Live!

Visit:
  - Vote: `http://<your-ec2-ip>:31000`
  - Results: `http://<your-ec2-ip>:31001`


![image alt](https://github.com/riyaj-2002/voting_app_project/blob/cea6d1d092e422995ae6ca56611fb64324fcc278/Screenshot%202025-04-29%20135413.png)

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20143024.png)

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20142957.png)

