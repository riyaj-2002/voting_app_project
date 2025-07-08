
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

## 1. Create a VPC
# Set the following:
   - **VPC Name:** `vpc-project`
   - **IPv4 CIDR block:** `10.0.0.0/24`
   - Set **Tenancy** as `Default`.
   - Click **Create VPC**.

  ![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20152216.png)   

## 2. Create Subnets
### Public Subnet:
# Set the following for the public subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pub-sub-project`
   - **CIDR block**: `10.0.0.1/24`
   - Click **Create Subnet**.

### Private Subnet:
# Set the following for the private subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pri-sub-project`
   - **CIDR block**: `10.0.0.128/24`
   - Click **Create Subnet**.

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-04-23%20204445.png)

## 3. Create an Internet Gateway (IGW)
# Set the following for the private subnet:
  - **IGW Name:** `igw-project`
# Attach the IGW:
   - Select the VPC and click **Attach**.

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-04-23%20204548.png)

## 4. Create a Route Table (RT)
# Set the following:
  - **RT Name:** `rt-project`
  - Add a route: `0.0.0.0/0` → Target: `igw-project`.
  - Associate the Route Table with the Public Subnet and Private Subnet

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-04-23%20204525.png)

---

## 🛠️ EC2 Instance Setup

## 1. Launch EC2 Instance
# Set the following:
   - **InstanceType:** t2.medium
   - **KeyName:** key.pair.pem
   - **VPC:** vpc-project
   - **SUBNET:** pub-sub-project
   - **SECURITY GROUP:** SSH (port 22) , HTTP (port 80) , HTTPS (port 443) 

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-04-29%20143622.png)


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

```

---

### 2. Clone the Voting App Repository

```bash
git clone https://github.com/riyaj-2002/voting_app_roject.git
cd voting_app_project
```

---

### 3. Run the Application

```bash
docker-compose up -d
```

This will start:
- Python app on port `8080`
- Node.js app on port `8081`

---

## 🌐 Access the App

```bash
curl ifconfig.me
```

---


##  See the Running Ports
```bash
docker ps
```
---

## 🛑 Stop the App (Optional)

```bash
docker compose down
```

---


## ✅ Application is Live!

Visit:
  - Vote: `http://<your-ec2-ip>:8080`
  - Results: `http://<your-ec2-ip>:8081`


![image alt](https://github.com/riyaj-2002/voting_app_project/blob/cea6d1d092e422995ae6ca56611fb64324fcc278/Screenshot%202025-04-29%20135413.png)

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20143024.png)

![image alt](https://github.com/riyaj-2002/voting_app_project/blob/d6f3aa6158fad53cf432cc1bc71930d9aa32dc04/Screenshot%202025-05-02%20142957.png)

