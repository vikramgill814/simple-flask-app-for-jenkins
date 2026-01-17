# 🚀 Simple Flask App CI/CD with Jenkins & Docker

This repository demonstrates a complete CI/CD pipeline using Jenkins, Docker, and a Python Flask application.

The pipeline automatically:
- Clones the repository
- Installs Python dependencies
- Builds and tests the Flask app
- Builds a Docker image
- Runs the application in a Docker container

---

## 🧰 Tech Stack

- **Python 3.9+**
- **Flask 3.0.0**
- **Jenkins LTS**
- **Docker**
- **GitHub**
- **Ubuntu 24.04**

---

## 📌 Project Overview

This project was created as part of a Jenkins pipeline assignment to demonstrate:
- Jenkins installation on Linux
- SCM-based pipeline configuration
- Docker integration with Jenkins
- Real-world CI/CD troubleshooting and fixes

---

## 🔧 Jenkins Setup (Server Side)

### Step 1: Jenkins Installation

Jenkins was installed on an Ubuntu 24.04 server using a shell script from this repository:

🔗 **Jenkins Setup Repository**
https://github.com/vikramgill814/setup-jenkins2.35-linux

Features of the Jenkins setup script:
- Installs OpenJDK 21
- Adds official Jenkins LTS repository
- Installs Jenkins via apt
- Enables and starts Jenkins service
- Prints initial admin password

---

## 🔐 Jenkins Initial Setup (Admin User)

After installation, Jenkins was accessed on:
```
http://<SERVER_IP>:8080
```

The initial admin password was retrieved using:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Then:
- Suggested plugins were installed
- Admin user was created
-Screenshots
<img width="674" height="640" alt="image" src="https://github.com/user-attachments/assets/075ef67e-6763-47a3-8096-5a9d1fae6a2e" />
<img width="689" height="494" alt="image" src="https://github.com/user-attachments/assets/04a8b062-245f-4fbd-9d27-f7c2103a64b4" />

---

## 🔁 Jenkins Pipeline Configuration

- A Pipeline job was created
- **Pipeline type:** Pipeline script from SCM
- **SCM:** Git
- **Repository URL:** https://github.com/vikramgill814/simple-flask-app-for-jenkins
- **Branch:** main
- Jenkinsfile was automatically detected from the repo

---

## 📂 Repository Structure

```
.
├── app.py                 # Flask application
├── requirements.txt       # Python dependencies
├── Dockerfile            # Docker image definition
├── Jenkinsfile          # Jenkins pipeline configuration
└── README.md            # This file
```

---

## ⚙️ Jenkins Pipeline Stages

The Jenkinsfile contains the following stages:

1. **Checkout** – Clones the repository from GitHub
2. **Install Dependencies** – Creates Python virtual environment and installs packages
3. **Build** – Compiles Python code and checks for syntax errors
4. **Test** – Runs application tests (placeholder for extended testing)
5. **Deploy** – Builds Docker image and runs container

---

## 🐳 Docker Permission Issue & Fix

### ❌ Problem

Pipeline initially failed with:
```
permission denied while trying to connect to the Docker daemon socket
unix:///var/run/docker.sock
```

### 🔍 Cause

- Docker runs as `root`
- Jenkins runs as `jenkins` user
- Jenkins user had no Docker permissions

### ✅ Solution

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
sudo systemctl restart jenkins
```

**Verification:**
```bash
sudo -u jenkins docker ps
```

✔ No permission error → Docker access fixed

---

## 🌐 Flask Application Port Issue & Fix

### ❌ Problem

Application container was running, but website was not accessible.

### 🔍 Cause

Flask was binding to `127.0.0.1` inside the container, making it inaccessible from outside.

### ✅ Fix

Updated `app.py`:
```python
app.run(host="0.0.0.0", port=5000, debug=True)
```

This binds Flask to all available network interfaces inside the container.

---

## 🌍 Running Application

### Locally (Without Docker)

1. Install dependencies:
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

2. Run the application:
```bash
python app.py
```

The application will be available at: `http://localhost:5000`

### With Docker

Build and run the Docker image:
```bash
docker build -t flask-jenkins-app .
docker run -d -p 5000:5000 flask-jenkins-app
```

The application will be accessible at: `http://<SERVER_IP>:5000`

**Note:** Ensure port 5000 is open in your server firewall / AWS security group.

---

## 📡 Application Endpoints

- `GET /` – Main page displaying the deployment message
- `GET /health` – Health check endpoint returning JSON status

---

## ✅ Pipeline Success

After fixing:
- Docker permissions
- Flask binding issue
- Python virtual environment configuration

The pipeline completed successfully and the application became accessible at the configured server IP.

---

## 🔒 Security Notes

- Adding Jenkins to the docker group allows Docker access
- This is standard practice for CI servers
- Suitable for:
  - Single-node Jenkins
  - EC2-based Jenkins

For production-grade security, consider:
- Docker agents
- Kubernetes orchestration
- Rootless Docker
- Firewall rules and VPC isolation

---

## 📦 Features

- Simple and responsive web application UI
- Real-time deployment status indicator
- Health check endpoint for monitoring
- Docker containerization
- Automated Jenkins CI/CD pipeline
- Python virtual environment isolation

---

## 🎯 Key Learnings

- Jenkins + Docker integration
- SCM-based pipelines
- Linux permissions troubleshooting
- Flask container networking
- Real-world CI/CD debugging
- Python virtual environment best practices
- Docker image building and container orchestration

---

## 👨‍💻 Author

**Vikramjeet Singh Gill**
- GitHub: https://github.com/vikramgill814

---

## ⭐ Acknowledgment

This project was created as part of a Jenkins CI/CD assignment to demonstrate practical DevOps skills.

If you like this project, feel free to ⭐ star the repository!
