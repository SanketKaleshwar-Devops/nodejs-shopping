# 🛒 Node.js Shopping Application - DevOps CI/CD Pipeline

![CI/CD](https://img.shields.io/badge/CI%2FCD-Jenkins-blue)
![Docker](https://img.shields.io/badge/Container-Docker-blue)
![Kubernetes](https://img.shields.io/badge/Orchestration-Kubernetes-blue)
![AWS](https://img.shields.io/badge/Cloud-AWS-orange)

## 🌐 Live Application
**App URL:** http://a386cd0e6fc5649d398d19b793d5ce3a-158000688.ap-south-1.elb.amazonaws.com

**Jenkins URL:** http://15.207.87.123:8080

**GitHub:** https://github.com/SanketKaleshwar-Devops/nodejs-shopping

---

## 📋 Project Overview

This project demonstrates a complete CI/CD pipeline for a Node.js e-commerce application. The pipeline automatically builds, tests, and deploys the application to AWS EKS (Elastic Kubernetes Service) whenever code is pushed to GitHub.

---

## 🏗️ Architecture
Developer → Git Push → GitHub → Jenkins Pipeline
→ Build Docker Image → Run Tests → Push to ECR
→ Deploy to EKS → AWS Load Balancer → Live App

---

## 🛠️ Tech Stack

### Application
| Technology | Purpose |
|---|---|
| Node.js | Backend runtime |
| Express.js | Web framework |
| EJS | Template engine |
| MongoDB | Database |
| Mongoose | ODM for MongoDB |

### DevOps Tools
| Tool | Purpose |
|---|---|
| Git & GitHub | Source code management |
| Jenkins | CI/CD automation |
| Docker | Containerization |
| Amazon ECR | Container registry |
| Amazon EKS | Kubernetes cluster |
| Amazon ELB | Load balancer |
| AWS IAM | Access management |
| kubectl | Kubernetes CLI |
| eksctl | EKS cluster management |

---

## 📁 Project Structure

    nodejs-shopping/
    ├── app.js
    ├── Dockerfile
    ├── Jenkinsfile
    ├── .env.example
    ├── package.json
    ├── k8s/
    │   ├── deployment.yaml
    │   ├── service.yaml
    │   └── mongodb.yaml
    ├── controllers/
    ├── models/
    ├── routes/
    ├── views/
    ├── public/
    └── middleware/
---

## 🔄 CI/CD Pipeline Stages

| Stage | Description |
|---|---|
| Checkout | Pull latest code from GitHub |
| Build Docker Image | Build and tag Docker image |
| Test | Run basic application tests |
| Push to ECR | Push image to Amazon ECR |
| Deploy to EKS | Deploy MongoDB + App to Kubernetes |
| Get App URL | Display live application URL |

---

## ☁️ AWS Infrastructure

### IAM
| Resource | Type | Purpose |
|---|---|---|
| jenkins-cicd | IAM User | Jenkins AWS access |
| eks-cluster-role | IAM Role | EKS cluster |
| eks-node-role | IAM Role | EKS worker nodes |

### Services Used
| Service | Details |
|---|---|
| EC2 | Jenkins server - t3.small - Ubuntu 22.04 |
| ECR | Repository: nodejs-shopping |
| EKS | Cluster: nodejs-shopping-cluster - K8s 1.34 |
| ELB | Auto-created by Kubernetes LoadBalancer service |

---

## 🐳 Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

---

## ☸️ Kubernetes Resources

| Resource | Type | Details |
|---|---|---|
| nodejs-shopping | Deployment | 1 replica - App |
| mongodb | Deployment | 1 replica - Database |
| nodejs-shopping-service | LoadBalancer | Port 80 → 3000 |
| mongodb-service | ClusterIP | Port 27017 |

---

## 🔧 Environment Variables

| Variable | Description |
|---|---|
| MONGODB_URI | MongoDB connection string |
| MONGO_USER | MongoDB username |
| MONGO_PWD | MongoDB password |
| MONGO_DB | Database name |
| PORT | Application port (default 3000) |

---

## 🚀 Run Locally

```bash
# Clone repo
git clone https://github.com/SanketKaleshwar-Devops/nodejs-shopping.git
cd nodejs-shopping

# Install dependencies
npm install

# Copy env file and fill values
cp .env.example .env

# Start app
npm start

# Visit
http://localhost:3000
```

---

## ✅ Application Features

- User Signup and Login
- Product listing and search
- Add to cart
- Order management
- Admin panel for product management
- Image upload for products

---

## 👨‍💻 Author

**Sanket Kaleshwar**
GitHub: [@SanketKaleshwar-Devops](https://github.com/SanketKaleshwar-Devops)
