# DevOps CI/CD Pipeline 🚀

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Docker](https://img.shields.io/badge/docker-ready-blue)]()
[![Kubernetes](https://img.shields.io/badge/kubernetes-enabled-326CE5)]()

> Automated CI/CD pipeline using Jenkins, Docker, and Kubernetes for continuous deployment of a Flask web application

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Pipeline Workflow](#pipeline-workflow)
- [Project Structure](#project-structure)
- [What I Learned](#what-i-learned)
- [Future Improvements](#future-improvements)
- [Contact](#contact)

## 🎯 Overview

This project demonstrates a complete CI/CD automation workflow that:
- Automatically triggers builds when code is pushed to GitHub
- Builds Docker images for a Python Flask application
- Deploys containerized application to a Kubernetes cluster
- Provides automated rollout of application updates

**Purpose:** To learn and demonstrate DevOps best practices including continuous integration, containerization, and orchestration.

## 🏗️ Architecture

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│              │         │              │         │              │
│    GitHub    │ ──────> │   Jenkins    │ ──────> │    Docker    │
│  (Webhook)   │         │  (Pipeline)  │         │    (Build)   │
│              │         │              │         │              │
└──────────────┘         └──────────────┘         └──────────────┘
                                                           │
                                                           │
                                                           v
                         ┌──────────────────────────────────────┐
                         │                                      │
                         │         Kubernetes Cluster          │
                         │                                      │
                         │  ┌─────────┐  ┌─────────┐           │
                         │  │  Pod 1  │  │  Pod 2  │           │
                         │  │  Flask  │  │  Flask  │           │
                         │  └─────────┘  └─────────┘           │
                         │                                      │
                         └──────────────────────────────────────┘
```

## 🛠️ Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| Jenkins | 2.x | CI/CD automation server |
| Docker | 20.10+ | Container runtime |
| Kubernetes | 1.28+ | Container orchestration |
| Python/Flask | 3.x | Sample web application |
| GitHub | - | Version control & webhooks |
| Docker Hub | - | Container image registry |

## ✨ Features

- ✅ **Automated Builds** - GitHub webhooks trigger pipeline automatically
- ✅ **Containerization** - Application packaged as Docker image
- ✅ **Orchestration** - Kubernetes manages container deployment
- ✅ **Version Control** - Git-based workflow for code management
- ✅ **Health Checks** - Kubernetes monitors application health
- ✅ **Scalability** - Easy to scale pods up or down

## 📦 Prerequisites

Before running this project, ensure you have:

```bash
# Required Software
- Docker Desktop (with Kubernetes enabled) OR
- Minikube for local Kubernetes cluster
- Jenkins (installed locally or via Docker)
- Git
- GitHub account
- Docker Hub account

# Required Knowledge
- Basic understanding of Docker containers
- Familiarity with Kubernetes concepts
- Basic Jenkins pipeline syntax
- Git version control
```

## 🚀 Setup Instructions

### Step 1: Clone the Repository
```bash
git clone https://github.com/rahul-thangavel/Devops-cicd-pipeline.git
cd Devops-cicd-pipeline
```

### Step 2: Set Up Local Kubernetes
```bash
# Option A: Enable Kubernetes in Docker Desktop
# Go to Docker Desktop → Settings → Kubernetes → Enable Kubernetes

# Option B: Using Minikube
minikube start
minikube status

# Verify cluster is running
kubectl cluster-info
kubectl get nodes
```

### Step 3: Install Jenkins
```bash
# Option A: Run Jenkins in Docker
docker run -d -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  --name jenkins jenkins/jenkins:lts

# Option B: Download and install Jenkins locally
# Visit: https://www.jenkins.io/download/

# Access Jenkins at: http://localhost:8080
# Get initial admin password:
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### Step 4: Configure Jenkins
```bash
# Install required Jenkins plugins:
1. Docker Pipeline
2. Kubernetes
3. GitHub Integration
4. Pipeline

# Set up credentials in Jenkins:
1. Go to Manage Jenkins → Manage Credentials
2. Add Docker Hub credentials (username/password)
3. Add GitHub personal access token
4. Add Kubernetes config (if needed)
```

### Step 5: Configure GitHub Webhook
```bash
# In your GitHub repository:
1. Go to Settings → Webhooks → Add webhook
2. Payload URL: http://your-jenkins-url:8080/github-webhook/
3. Content type: application/json
4. Select: Just the push event
5. Active: ✓
6. Add webhook
```

### Step 6: Create Jenkins Pipeline
```bash
1. In Jenkins, create new Pipeline job
2. Configure:
   - Source: Git
   - Repository URL: https://github.com/rahul-thangavel/Devops-cicd-pipeline.git
   - Script Path: Jenkinsfile
3. Save
```

### Step 7: Test the Pipeline
```bash
# Make a code change and push
git add .
git commit -m "Test pipeline trigger"
git push origin main

# Watch Jenkins build automatically
# Check Kubernetes deployment:
kubectl get pods
kubectl get services
```

## 🔄 Pipeline Workflow

The Jenkins pipeline executes these stages:

1. **Checkout** 
   - Pulls latest code from GitHub repository
   - Triggered by webhook on git push

2. **Build**
   - Builds Docker image from Dockerfile
   - Tags image with build number

3. **Test** (Optional)
   - Runs unit tests on application
   - Validates container health

4. **Push**
   - Pushes Docker image to Docker Hub
   - Makes image available for deployment

5. **Deploy**
   - Updates Kubernetes deployment
   - Rolls out new pods with updated image
   - Waits for pods to be ready

6. **Verify**
   - Checks deployment status
   - Verifies pods are running
   - Reports success/failure

## 📁 Project Structure

```
Devops-cicd-pipeline/
├── app.py                 # Flask application code
├── requirements.txt       # Python dependencies
├── Dockerfile            # Docker image definition
├── Jenkinsfile           # Jenkins pipeline configuration
├── k8s/
│   ├── deployment.yaml  # Kubernetes deployment manifest
│   └── service.yaml     # Kubernetes service manifest
├── tests/               # Application tests (optional)
│   └── test_app.py
└── README.md            # This file
```

## 🧠 What I Learned

### Technical Skills
- **Jenkins Pipeline Scripting**: Learned to write declarative pipelines with Groovy syntax
- **Docker Best Practices**: Multi-stage builds, layer optimization, .dockerignore usage
- **Kubernetes Deployments**: Understanding pods, services, deployments, and rolling updates
- **Webhook Integration**: Configured GitHub webhooks for event-driven automation
- **CI/CD Concepts**: Continuous integration and deployment workflow design

### Troubleshooting Experience
- **Issue**: Jenkins couldn't connect to Kubernetes cluster
  - **Solution**: Configured proper kubeconfig path in Jenkins credentials
  
- **Issue**: Docker build failing with permission errors
  - **Solution**: Added Jenkins user to Docker group and restarted Jenkins

- **Issue**: Pods not pulling latest image
  - **Solution**: Used imagePullPolicy: Always in Kubernetes deployment

- **Issue**: GitHub webhook not triggering builds
  - **Solution**: Exposed Jenkins via ngrok for local development, verified webhook payload

## 🎓 Key Takeaways

1. **Automation Reduces Errors**: Eliminated 12 manual deployment steps, reducing human error
2. **Infrastructure as Code**: Kubernetes manifests make deployments reproducible
3. **Version Control Everything**: Jenkinsfile in repo means pipeline is version-controlled
4. **Container Benefits**: Consistent environments from development to production

## 🚀 Future Improvements

- [ ] Add comprehensive test suite with pytest
- [ ] Implement staging environment before production
- [ ] Add monitoring with Prometheus and Grafana
- [ ] Implement blue-green or canary deployment strategies
- [ ] Add automated rollback on deployment failure
- [ ] Integrate security scanning (Trivy, Snyk)
- [ ] Add Slack/email notifications for build status
- [ ] Implement multi-branch pipeline for feature branches

## 🤝 Contributing

This is a learning project, but feedback and suggestions are welcome! Feel free to:
- Open an issue for bugs or questions
- Submit pull requests for improvements
- Share your own implementation ideas

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 📫 Contact

**Rahul Thangavel**
- 💼 LinkedIn: [linkedin.com/in/rahul-thangavel](https://linkedin.com/in/rahul-thangavel)
- 📧 Email: rahult30112002@gmail.com
- 🐙 GitHub: [@rahul-thangavel](https://github.com/rahul-thangavel)

---

⭐ **If this project helped you learn about CI/CD, please consider giving it a star!**
