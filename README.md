CICD-PROJECT:GitHub_Jenkins_SonarQube_Docker<br>
![image](https://github.com/SivaranjanAsokan/3.1.DEVOPS_CICD_Project_2_Jen_SQ_Doc_Host/assets/163242501/d858b3ac-6223-4373-b996-8a0c79e4edb8)

Here's a comprehensive `README.md` file based on your detailed instructions:

# SonarQube, Jenkins, and Docker Integration Guide

This guide covers the steps to set up and integrate SonarQube, Jenkins, and Docker on Ubuntu servers.

## SonarQube Setup

### SonarQube Server

#### Step 1: Install JDK

```bash
sudo apt install openjdk-17-jre
```

#### Step 2: Download and Install SonarQube
```bash
wget <sonarqube file link>
ls
sudo apt install unzip
unzip sonarqube.zip
cd sonarqube
cd bin
cd linux
./sonar.sh console
```

#### Step 3: Configure Firewall
Add inbound rule for port 9000 to allow SonarQube traffic.

## SonarQube Configuration

### SonarQube GUI

#### Step 1: Access SonarQube
Open a browser and navigate to `http://<your-ip>:9000`

Login: `admin`
Password: `admin`

#### Step 2: Set New Password
Change the default password.

#### Step 3: Create a Project
1. Navigate to "Create Project" -> Manual
2. Enter project details:
   - Name
   - Key
   - Branch: `main`

#### Step 4: Setup Project
1. CI: Select `Jenkins`
2. Platform: `GitHub`
3. Build Description: `other`
4. Copy the generated Project Key.

#### Step 5: Generate Token
1. Navigate to Admin user -> Security
2. Generate a token and note it down.

## Jenkins Setup

### Jenkins Server

#### Step 1: Install Java & Jenkins
Follow the Jenkins installation guide for Ubuntu.

#### Step 2: Create Free-style Job
1. Source: Git URL
2. Branch: `*/main`

#### Step 3: Build Trigger
Set up GitHub webhook.

#### Step 4: Manage Plugins
Install the following plugins:
- SonarQube Scanner
- SSH2 Easy

#### Step 5: Global Tool Configuration
1. Go to SonarScanner
2. Add a new SonarScanner:
   - Name: `SonarScanner`
   - Install automatically: `Yes` (version 4.8.0)

#### Step 6: Configure System
1. Go to SonarQube servers
2. Add a new SonarQube server:
   - Name
   - URL
   - Authentication: Secret text (use the token from SonarQube)

#### Step 7: Build Steps
1. Add "Execute SonarQube Scanner" build step
2. Analysis Properties: Add your Project Key

Run the build to check the SonarQube scan.

## Docker Setup

### Docker Server (Ubuntu)

#### Step 1: Install Docker
Follow the Docker installation guide for Ubuntu.

#### Step 2: Enable SSH and Set Password
```bash
sudo su
vi /etc/ssh/sshd_config
# Enable PubkeyAuthentication and PasswordAuthentication
systemctl restart sshd
passwd ubuntu  # set new password
```

#### Step 3: Create Deployment Directory
```bash
mkdir /home/ubuntu/website
sudo usermod -aG docker ubuntu
newgrp docker
```

## Integrate Jenkins with Docker

### Jenkins Server

#### Step 1: Switch User to Jenkins
```bash
su jenkins
```

#### Step 2: Set up SSH Access to Docker Server
```bash
ssh ubuntu@<docker-server-ip>  # ensure connection works
ssh-keygen
ssh-copy-id ubuntu@<docker-server-ip>
ssh ubuntu@<docker-server-ip>
```

## GitHub Setup

### Create Webhook URL

## Jenkins Job Configuration

### Configure System

#### Step 1: Add Docker Server
1. Go to Server group
2. Add Server group:
   - Name: Docker server
   - SSH port: 22
   - Username: `ubuntu`
   - Password: `your-password`

#### Step 2: Add Server List
1. Server group name: Docker server
2. Server name: `docker`
3. Server IP: `dockerip`

### Post Build Action

#### Step 3: Add Remote Shell
1. Select target server: `docker`
2. Shell command: `touch hello.txt`

Run the build and check `hello.txt` on the Docker server.

### GitHub

#### Step 4: Create Dockerfile in GitHub
```Dockerfile
FROM nginx
COPY . /usr/share/nginx/html
```

Run the build to deploy.

### Jenkins GUI

#### Step 5: Post-build Actions

1. Remove remote shell.

#### Step 6: Execute Shell Command
```bash
scp -r ./* ubuntu@<docker-server-ip>:/home/ubuntu/website
```

Run the build.

#### Step 7: Add Remote Shell for Docker Commands
```bash
cd /home/ubuntu/website
docker build -t mywebsite .
docker run -itd -p 8085:80 --name onix mywebsite
```

Run the build.

---

This README provides step-by-step instructions to set up and integrate SonarQube, Jenkins, and Docker. Follow these instructions carefully to ensure a successful setup.
