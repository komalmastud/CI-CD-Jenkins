# CI/CD Pipeline with Jenkins, SonarQube, and Docker

## Project Overview
This project demonstrates how to set up a CI/CD pipeline using Jenkins, GitHub, SonarQube, and Docker on an AWS EC2 instance. The pipeline automates the process of building, testing, analyzing code quality, and deploying applications.

## Tools & Technologies Used
- Jenkins: CI/CD automation server
- SonarQube: Static code analysis tool
- Docker: Containerization platform
- GitHub: Version control and repository management
- AWS EC2: Cloud-based infrastructure

## Project Setup

### 1. Create AWS EC2 Instances
Set up three Ubuntu-based EC2 instances:
- Jenkins Server
- SonarQube Server
- Docker Server

### 2. Install Jenkins
```bash
sudo apt update -y
sudo apt install openjdk-21-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
echo "deb https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt update -y
sudo apt install jenkins -y
sudo systemctl start jenkins
```
Access Jenkins: `http://your-ec2-ip:8080`

### 3. Install and Configure SonarQube
```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:lts
```
Access SonarQube: `http://your-ec2-ip:9000`

### 4. Set Up Jenkins Pipeline

#### Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'docker build -t my-app .'
                sh 'docker run my-app npm test'
            }
        }
        stage('Code Analysis') {
            steps {
                sh 'sonar-scanner -Dsonar.projectKey=my-app'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 80:80 my-app'
            }
        }
    }
}
```

### 5. Configure GitHub Webhooks
1. Go to GitHub Repository → Settings → Webhooks
2. Add a new webhook:
   - Payload URL: `http://your-jenkins-ip:8080/github-webhook/`
   - Content type: `application/json`
   - Select **Just the push event**

### 6. Trigger CI/CD Pipeline
Push a code change to GitHub to trigger the pipeline:
```bash
echo "Test CI/CD Pipeline" > test.txt
git add .
git commit -m "Trigger CI/CD"
git push origin main
```

## CI/CD Workflow
1. Developer pushes code to GitHub.
2. Jenkins automatically triggers the pipeline.
3. SonarQube performs code quality analysis.
4. Docker builds and deploys the application.
5. The latest version of the application is deployed.

## Future Enhancements
- Implement GitHub Actions for CI/CD automation.
- Deploy using Kubernetes.
- Use AWS ECS or Lambda for serverless deployments.

## Contact
- GitHub: [your-username](https://github.com/your-username)
- LinkedIn: [your-profile](https://linkedin.com/in/your-profile)

This README provides a structured approach to setting up a CI/CD pipeline with Jenkins, SonarQube, and Docker on AWS EC2. Modify the repository links and configuration details as per your project setup.
