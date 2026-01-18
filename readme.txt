README.md – Automated Website Deployment Project

# 🚀 Automated Website Deployment Project

This project demonstrates a **fully automated website deployment** using:

- **Linux (Ubuntu 22.04)**
- **Nginx Web Server**
- **GitHub** (version control)
- **Jenkins** (CI/CD automation)
- **GitHub Webhook** (automatic deployment on push)

---

## 📂 Folder Structure

mywebsite/
├── index.html
├── style.css # Optional styling
├── README.md # Project info
└── Jenkinsfile # Pipeline script

yaml
Copy code

---

## 🛠 Step 0: Prerequisites

1. Ubuntu 22.04 server with SSH access
2. GitHub account
3. Jenkins installed on server (see Step 3)

---

## 🏗 Step 1: Create Website Files

```bash
mkdir ~/mywebsite
cd ~/mywebsite

# index.html
cat <<EOL > index.html
<!DOCTYPE html>
<html>
<head>
    <title>My DevOps Project</title>
</head>
<body>
    <h1>Hello, Arshit! Website deployed via Jenkins 🚀</h1>
</body>
</html>
EOL

# Optional: style.css
echo "body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }" > style.css

# Jenkinsfile
cat <<EOL > Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Pull Code from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/mywebsite.git'
            }
        }
        stage('Deploy to Server') {
            steps {
                sh '''
                sudo rm -rf /var/www/mywebsite/*
                sudo cp -r * /var/www/mywebsite/
                '''
            }
        }
    }
    post {
        success { echo 'Website deployed successfully! 🚀' }
        failure { echo 'Deployment failed!' }
    }
}
EOL

💻 Step 2: Push Code to GitHub
bash
Copy code
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<username>/mywebsite.git
git push -u origin main

🖥 Step 3: Linux Server Setup
bash
Copy code
ssh ubuntu@<server-ip>

# Update server & install Nginx
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx

# Create website directory
sudo mkdir -p /var/www/mywebsite
sudo chown -R $USER:$USER /var/www/mywebsite
Test in browser: http://<server-ip>

🔧 Step 4: Install Jenkins
bash
Copy code
sudo apt install openjdk-11-jdk -y

# Add Jenkins repo
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

# Install Jenkins
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Get initial password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Access Jenkins: http://<server-ip>:8080

Install suggested plugins → Create admin user

📦 Step 5: Create Jenkins Pipeline Job
Jenkins → New Item → Pipeline → Name: Website-Deploy
Pipeline → Use Jenkinsfile from repo
Build Now → Check console log → Browser: http://<server-ip>/mywebsite

🔄 Step 6: Setup GitHub Webhook for Automation
GitHub → Repo → Settings → Webhooks → Add webhook
Payload URL: http://<server-ip>:8080/github-webhook/
Content type: application/json
Trigger: Just the push event
Save → Any push → Jenkins auto-build & deploy

✅ Project Complete
GitHub → Version control
Linux → Web server
Jenkins → CI/CD automation
Webhook → Auto-deployment
Browser → Live website

