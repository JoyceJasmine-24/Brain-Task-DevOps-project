# Brain Tasks DevOps Deployment Project

## Project Overview

This project demonstrates the deployment of the Brain Tasks React application using DevOps practices and AWS cloud services.

The application was containerized using Docker, stored in AWS ECR, and deployed to AWS EKS using Kubernetes manifests.

---

# Application Details

- Application Name: Brain Tasks
- Frontend Framework: React
- Containerization: Docker
- Container Registry: AWS ECR
- Kubernetes Platform: AWS EKS
- CI/CD Tool: AWS CodeBuild
- Version Control: GitHub
- Monitoring: CloudWatch Logs

Repository Used:
https://github.com/Vennilavanguvi/Brain-Tasks-App.git

---

# Project Architecture

GitHub Repository
↓
AWS CodeBuild
↓
Docker Build
↓
Push Image to AWS ECR
↓
Deploy to AWS EKS
↓
Access Application via LoadBalancer

---

# Step 1 — Clone Repository

```bash
git clone https://github.com/Vennilavanguvi/Brain-Tasks-App.git
cd Brain-Tasks-App
```

---

# Step 2 — Dockerize Application

Created a Dockerfile for containerizing the React application.

## Dockerfile

```dockerfile
FROM nginx:alpine

COPY dist/ /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# Step 3 — Build Docker Image

```bash
docker build -t brain-task-app .
```

Check Docker Images:

```bash
docker images
```

---

# Step 4 — Create AWS ECR Repository

Created ECR repository named:

```text
brain-task-app
```

Login to ECR:

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 145689194017.dkr.ecr.ap-south-1.amazonaws.com
```

Tag Docker Image:

```bash
docker tag brain-task-app:latest 145689194017.dkr.ecr.ap-south-1.amazonaws.com/brain-task-app:latest
```

Push Image to ECR:

```bash
docker push 145689194017.dkr.ecr.ap-south-1.amazonaws.com/brain-task-app:latest
```

---

# Step 5 — Kubernetes Deployment

Created Kubernetes deployment and service YAML files.

## deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: brain-task-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: brain-task-app
  template:
    metadata:
      labels:
        app: brain-task-app
    spec:
      containers:
      - name: brain-task-container
        image: 145689194017.dkr.ecr.ap-south-1.amazonaws.com/brain-task-app:latest
        ports:
        - containerPort: 80
```

---

## service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: brain-task-service
spec:
  selector:
    app: brain-task-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

---

# Step 6 — Deploy Application to EKS

Apply Deployment:

```bash
kubectl apply -f deployment.yaml
```

Apply Service:

```bash
kubectl apply -f service.yaml
```

Check Pods:

```bash
kubectl get pods
```

Check Services:

```bash
kubectl get svc
```

---

# Step 7 — CodeBuild Configuration

Created buildspec.yml file for CI/CD automation.

## buildspec.yml

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR
      - aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 145689194017.dkr.ecr.ap-south-1.amazonaws.com

  build:
    commands:
      - docker build -t brain-task-app .
      - docker tag brain-task-app:latest 145689194017.dkr.ecr.ap-south-1.amazonaws.com/brain-task-app:latest

  post_build:
    commands:
      - docker push 145689194017.dkr.ecr.ap-south-1.amazonaws.com/brain-task-app:latest
```

---

# Step 8 — Version Control

Initialized Git repository and pushed code using CLI commands.

```bash
git init
git add .
git commit -m "DevOps project completed"
git remote add origin https://github.com/JoyceJasmine-24/Brain-Task-DevOps-project.git
git push -u origin main
```

---

# Step 9 — Application Deployment URL

Application successfully deployed using AWS EKS LoadBalancer.

## LoadBalancer URL

```text
ab810a4d564a84efdb19d1af688d5ee5-1798423270.ap-south-1.elb.amazonaws.com
```

---

# Monitoring

AWS CloudWatch Logs used for:
- Build monitoring
- Deployment monitoring
- Application monitoring

---

# Screenshots

Project screenshots are available inside the Screenshots folder.

Included Screenshots:
- Git Clone
- Docker Build
- Docker Images
- ECR Repository
- Docker Push
- Kubernetes Pods
- Kubernetes Services
- Running Application
- CodeBuild Project
- GitHub Repository

---

# Technologies Used

- React
- Docker
- AWS ECR
- AWS EKS
- Kubernetes
- AWS CodeBuild
- GitHub
- CloudWatch

---

# Project Status

Project Successfully Completed ✅
