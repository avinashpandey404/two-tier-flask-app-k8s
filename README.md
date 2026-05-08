# 🚀 Two-Tier Application Deployment on AWS EKS (Flask + HTML + MySQL)

# 📌 Project Introduction

This project demonstrates a complete end-to-end deployment of a two-tier application on AWS EKS (Elastic Kubernetes Service).

The project contains:

* Flask backend application
* HTML frontend page
* MySQL database
* Docker containerization
* Kubernetes deployments
* AWS Application Load Balancer (ALB)
* Kubernetes Ingress
* Persistent storage for MySQL
* Kubernetes networking
* Cloud-native deployment architecture

The entire application is deployed inside a Kubernetes cluster running on AWS EKS.

---

# 🏗️ What is Two-Tier Architecture?

A two-tier architecture separates the application into two layers:

| Layer             | Purpose                          |
| ----------------- | -------------------------------- |
| Application Layer | Handles frontend + backend logic |
| Database Layer    | Stores application data          |

In this project:

* Flask handles frontend and backend logic
* MySQL handles database storage

---

# 🏗️ Complete Architecture

```text
                    Users
                       ↓
            AWS Application Load Balancer
                       ↓
                Kubernetes Ingress
                       ↓
               Flask Kubernetes Service
                       ↓
               Flask Application Pods
                       ↓
               MySQL Kubernetes Service
                       ↓
                    MySQL Pod
                       ↓
              Persistent Volume Storage
```

---

# 🔄 Complete Project Workflow

```text
Developer Creates Application
            ↓
Docker Images Created
            ↓
Images Pushed to DockerHub
            ↓
AWS EKS Cluster Created
            ↓
kubectl Connected to Cluster
            ↓
MySQL Deployment Created
            ↓
Persistent Storage Attached
            ↓
Flask Application Deployment Created
            ↓
Kubernetes Services Created
            ↓
Ingress Created
            ↓
AWS ALB Automatically Created
            ↓
Users Access Application Publicly
```

---

# 🛠️ Technologies Used

| Technology | Purpose                        |
| ---------- | ------------------------------ |
| AWS EKS    | Managed Kubernetes Service     |
| Docker     | Containerization               |
| Kubernetes | Container Orchestration        |
| Flask      | Backend Framework              |
| HTML/CSS   | Frontend UI                    |
| MySQL      | Database                       |
| AWS ALB    | Load Balancer                  |
| kubectl    | Kubernetes Management Tool     |
| eksctl     | EKS Cluster Creation Tool      |
| YAML       | Kubernetes Configuration Files |
| EBS Volume | Persistent Storage             |

---

# 📁 Project Structure

```text
eks-two-tier-app/
│
├── app.py
├── requirements.txt
├── Dockerfile
│
├── templates/
│   └── index.html
│
├── k8s/
│   ├── flask-deployment.yaml
│   ├── flask-service.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── pvc.yaml
│   └── ingress.yaml
│
└── README.md
```

---

# 🚀 STEP 1 — Launch EC2 Instance

# 🔍 Why We Need EC2?

We need a Linux server to:

* Install Docker
* Install kubectl
* Install eksctl
* Configure AWS CLI
* Create EKS cluster
* Deploy Kubernetes resources

This EC2 machine acts as:

```text
Kubernetes Management Server
```

---

# 🔹 Launch EC2 Instance

Go to:

```text
AWS Console → EC2 → Launch Instance
```

---

# 🔹 Configuration

| Setting       | Value        |
| ------------- | ------------ |
| Name          | eks-server   |
| AMI           | Ubuntu 22.04 |
| Instance Type | t2.medium    |

---

# 🔍 Why t2.medium?

EKS tools consume memory.

We install:

* Docker
* kubectl
* eksctl
* AWS CLI

A t2.micro may crash due to low RAM.

---

# 🔓 Configure Security Group

Allow:

| Port | Purpose    |
| ---- | ---------- |
| 22   | SSH Access |
| 80   | HTTP       |
| 443  | HTTPS      |

---

# ❌ Possible Errors

## Error

```bash
Connection timed out
```

## Reason

Port 22 blocked.

## Fix

Allow SSH in Security Group.

---

# 🎯 Interview Questions

## Q1. Why did you use EC2 in this project?

```text
EC2 was used as a Kubernetes management server to install Docker, kubectl, eksctl, and deploy Kubernetes resources into the EKS cluster.
```

---

# 🚀 STEP 2 — Connect to EC2

## Command

```bash
ssh -i key.pem ubuntu@EC2-PUBLIC-IP
```

---

# 🔍 Command Explanation

| Part          | Purpose                 |
| ------------- | ----------------------- |
| ssh           | Secure shell connection |
| -i            | Identity key            |
| key.pem       | AWS private key         |
| ubuntu        | Default Ubuntu username |
| EC2-PUBLIC-IP | Public server IP        |

---

# ❌ Possible Error

## Error

```bash
Permission denied (publickey)
```

## Fix

```bash
chmod 400 key.pem
```

---

# 🚀 STEP 3 — Update Linux Server

## Command

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 🔍 Why We Update Server?

Updating ensures:

* Latest security patches
* Updated package versions
* Better compatibility
* Stable installations

---

# 🔍 Command Explanation

| Command     | Purpose                    |
| ----------- | -------------------------- |
| apt update  | Refresh package list       |
| apt upgrade | Upgrade installed packages |
| -y          | Auto approve               |

---

# 🚀 STEP 4 — Install Docker

# 🔍 What is Docker?

Docker is a containerization platform.

It packages:

* Application code
* Dependencies
* Libraries
* Runtime

into a single portable container.

---

# 🔍 Why We Use Docker?

Without Docker:

* Application may fail on different systems
* Dependency conflicts occur
* Manual setup required

With Docker:

* Same environment everywhere
* Portable deployment
* Easy scaling
* Faster deployment

---

# 🔹 Install Docker

```bash
sudo apt install docker.io -y
```

---

# 🔹 Start Docker

```bash
sudo systemctl start docker
```

---

# 🔹 Enable Docker

```bash
sudo systemctl enable docker
```

---

# 🔹 Verify Docker

```bash
docker --version
```

---

# 🔍 What Happens Internally?

Docker daemon starts in background.

It manages:

* Images
* Containers
* Networks
* Volumes

---

# ❌ Possible Errors

## Error

```bash
docker: command not found
```

## Fix

Install Docker properly.

---

## Error

```bash
permission denied while trying to connect to docker daemon
```

## Fix

```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

---

# 🎯 Interview Questions

## Q1. Why did you use Docker?

```text
Docker was used to containerize the application and package all dependencies into portable containers for consistent deployment across environments.
```

---

# 🚀 STEP 5 — Install kubectl

# 🔍 What is kubectl?

kubectl is a command-line tool used to manage Kubernetes clusters.

It communicates with Kubernetes API Server.

---

# 🔍 Why We Use kubectl?

Using kubectl we can:

* Create deployments
* Create services
* View pods
* Scale applications
* Check logs
* Debug applications

---

# 🔹 Download kubectl

```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
```

---

# 🔹 Give Permission

```bash
chmod +x kubectl
```

---

# 🔹 Move Binary

```bash
sudo mv kubectl /usr/local/bin/
```

---

# 🔹 Verify Installation

```bash
kubectl version --client
```

---

# 🔍 What Happens Internally?

kubectl sends API requests to:

```text
Kubernetes API Server
```

The API server controls cluster operations.

---

# 🎯 Interview Questions

## Q1. What is kubectl?

```text
kubectl is a command-line tool used to manage Kubernetes clusters and communicate with the Kubernetes API server.
```

---

# 🚀 STEP 6 — Install eksctl

# 🔍 What is eksctl?

eksctl is an official CLI tool for creating and managing EKS clusters.

---

# 🔍 Why We Use eksctl?

Without eksctl:

* EKS setup becomes complex
* Manual VPC setup required
* Manual node group creation required

eksctl automates:

* VPC creation
* Worker nodes
* Security groups
* IAM roles
* Networking

---

# 🔹 Install eksctl

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

---

# 🔹 Move Binary

```bash
sudo mv /tmp/eksctl /usr/local/bin
```

---

# 🔹 Verify Installation

```bash
eksctl version
```

---

# 🎯 Interview Questions

## Q1. Why use eksctl?

```text
eksctl simplifies EKS cluster creation by automatically configuring networking, IAM, worker nodes, and Kubernetes infrastructure.
```

---

# 🚀 STEP 7 — Configure AWS CLI

# 🔍 Why AWS CLI?

AWS CLI allows EC2 server to interact with AWS services.

---

# 🔹 Install AWS CLI

```bash
sudo apt install awscli -y
```

---

# 🔹 Configure AWS CLI

```bash
aws configure
```

Enter:

* AWS Access Key
* AWS Secret Key
* Region
* Output Format

---

# 🔍 What Happens Internally?

AWS credentials are stored inside:

```text
~/.aws/
```

---

# ❌ Possible Errors

## Error

```bash
Unable to locate credentials
```

## Fix

Run:

```bash
aws configure
```

again.

---

# 🚀 STEP 8 — Create EKS Cluster

# 🔍 What is EKS?

Amazon EKS is a managed Kubernetes service.

AWS manages:

* Kubernetes Control Plane
* API Server
* etcd
* Availability
* Security

We manage:

* Worker Nodes
* Applications
* Deployments

---

# 🔍 Kubernetes Architecture

```text
                 Kubernetes Control Plane
                 ┌─────────────────────┐
                 │ API Server          │
                 │ Scheduler           │
                 │ Controller Manager  │
                 │ etcd Database       │
                 └─────────────────────┘
                           ↓
               ┌─────────────────────┐
               │ Worker Node         │
               │ kubelet             │
               │ kube-proxy          │
               │ Container Runtime   │
               └─────────────────────┘
```

---

# 🔹 Create Cluster

```bash
eksctl create cluster \
--name flask-cluster \
--region ap-south-1 \
--nodegroup-name workers \
--node-type t2.medium \
--nodes 2
```

---

# 🔍 Command Explanation

| Argument         | Purpose                |
| ---------------- | ---------------------- |
| create cluster   | Create EKS cluster     |
| --name           | Cluster name           |
| --region         | AWS region             |
| --nodegroup-name | Worker node group      |
| --node-type      | EC2 instance type      |
| --nodes 2        | Number of worker nodes |

---

# 🔍 What Happens Internally?

eksctl automatically creates:

* VPC
* Subnets
* Route tables
* IAM roles
* Security groups
* Worker nodes
* EKS cluster

---

# ⏳ Creation Time

Usually:

```text
15–25 minutes
```

---

# ❌ Possible Errors

## Error

```bash
Insufficient capacity
```

## Fix

Use different region or instance type.

---

# 🎯 Interview Questions

## Q1. What is EKS?

```text
Amazon EKS is a managed Kubernetes service that simplifies Kubernetes cluster management on AWS.
```

---

# 🚀 STEP 9 — Verify Cluster

## Command

```bash
kubectl get nodes
```

---

# 🔍 What Happens Internally?

kubectl contacts:

```text
Kubernetes API Server
```

The API server returns cluster node information.

---

# ✅ Output

```text
2 Ready nodes
```

---

# 🚀 STEP 10 — Create Flask Application

# 🔍 Why Flask?

Flask is lightweight and simple.

It is commonly used for:

* APIs
* Backend services
* Web applications

---

# 📄 app.py

```python
from flask import Flask, render_template
import mysql.connector

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

# 🔍 Code Explanation

| Code            | Purpose                  |
| --------------- | ------------------------ |
| Flask()         | Create Flask application |
| @app.route('/') | Homepage route           |
| render_template | Render HTML page         |
| host='0.0.0.0'  | Allow external access    |
| port=5000       | Application port         |

---

# 🔍 Why host='0.0.0.0'?

Without this:

```text
Container cannot accept external traffic
```

---

# 📄 requirements.txt

```text
flask
mysql-connector-python
```

---

# 🔍 Why requirements.txt?

This file stores Python dependencies.

Docker installs dependencies automatically.

---

# 📄 templates/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>EKS Project</title>
</head>
<body>

<h1>🚀 Two-Tier Application on AWS EKS</h1>

<p>Flask + HTML + MySQL + Kubernetes</p>

</body>
</html>
```

---

# 🔍 Why templates Folder?

Flask automatically looks inside:

```text
templates/
```

for HTML files.

---

# 🚀 STEP 11 — Create Dockerfile

# 🔍 What is Dockerfile?

Dockerfile contains instructions for building Docker image.

---

# 📄 Dockerfile

```dockerfile
FROM python:3.11

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```

---

# 🔍 Dockerfile Line-by-Line Explanation

## FROM python:3.11

Uses official Python image.

---

## WORKDIR /app

Creates working directory.

---

## COPY . .

Copies project files into container.

---

## RUN pip install

Installs dependencies.

---

## EXPOSE 5000

Application port.

---

## CMD

Runs Flask app.

---

# 🚀 STEP 12 — Build Docker Image

## Command

```bash
docker build -t flask-eks-app .
```

---

# 🔍 What Happens Internally?

Docker reads Dockerfile step-by-step.

Creates:

```text
Docker Image
```

---

# 🚀 STEP 13 — Push Image to DockerHub

# 🔍 Why DockerHub?

Kubernetes worker nodes pull images from DockerHub.

---

# 🔹 Login

```bash
docker login
```

---

# 🔹 Tag Image

```bash
docker tag flask-eks-app username/flask-eks-app
```

---

# 🔹 Push Image

```bash
docker push username/flask-eks-app
```

---

# ❌ Possible Errors

## Error

```bash
denied: requested access to the resource is denied
```

## Fix

Verify DockerHub username.

---

# 🚀 STEP 14 — Kubernetes YAML Files

# 🔍 Why YAML Files?

Kubernetes uses YAML manifests to define resources.

Using YAML we define:

* Deployments
* Services
* Ingress
* Volumes
* Configurations

---

# 🚀 STEP 15 — Create MySQL Deployment

# 🔍 What is Deployment?

Deployment manages:

* Pod creation
* Scaling
* Updates
* Self-healing

---

# 📄 mysql-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: mysql

spec:
  replicas: 1

  selector:
    matchLabels:
      app: mysql

  template:
    metadata:
      labels:
        app: mysql

    spec:
      containers:
      - name: mysql
        image: mysql:8

        env:
        - name: MYSQL_ROOT_PASSWORD
          value: password

        ports:
        - containerPort: 3306
```

---

# 🔍 YAML Deep Explanation

## apiVersion

Kubernetes API version.

---

## kind: Deployment

Creates deployment resource.

---

## replicas: 1

One MySQL pod.

---

## selector

Connects deployment with pods.

---

## image: mysql:8

Uses official MySQL image.

---

## MYSQL_ROOT_PASSWORD

Sets MySQL password.

---

# 🚀 STEP 16 — Deploy MySQL

## Command

```bash
kubectl apply -f mysql-deployment.yaml
```

---

# 🔍 What Happens Internally?

Kubernetes scheduler selects node.

Pod created inside worker node.

Container starts inside pod.

---

# 🚀 STEP 17 — Create MySQL Service

# 🔍 Why Service?

Pods are temporary.

Pod IP changes when restarted.

Service provides:

* Stable networking
* Fixed DNS name
* Internal communication

---

# 📄 mysql-service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: mysql-service

spec:
  selector:
    app: mysql

  ports:
  - port: 3306
    targetPort: 3306
```

---

# 🔍 How Service Works?

Service dynamically routes traffic to matching pods.

Using labels:

```text
app: mysql
```

---

# 🔍 Internal DNS

Flask app connects using:

```text
mysql-service
```

instead of pod IP.

---

# 🚀 STEP 18 — Deploy MySQL Service

## Command

```bash
kubectl apply -f mysql-service.yaml
```

---

# 🚀 STEP 19 — Create Flask Deployment

# 📄 flask-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: flask-app

spec:
  replicas: 2

  selector:
    matchLabels:
      app: flask-app

  template:
    metadata:
      labels:
        app: flask-app

    spec:
      containers:
      - name: flask-app
        image: username/flask-eks-app

        ports:
        - containerPort: 5000
```

---

# 🔍 Why replicas: 2?

Provides:

* High availability
* Load balancing
* Better fault tolerance

---

# 🔍 What Happens Internally?

Kubernetes creates:

```text
2 Flask Pods
```

across worker nodes.

---

# 🚀 STEP 20 — Deploy Flask Application

```bash
kubectl apply -f flask-deployment.yaml
```

---

# 🚀 STEP 21 — Create Flask Service

# 🔍 Why Flask Service?

Service exposes Flask pods internally.

Provides:

* Stable endpoint
* Load balancing
* Pod communication

---

# 📄 flask-service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: flask-service

spec:
  type: NodePort

  selector:
    app: flask-app

  ports:
  - port: 5000
    targetPort: 5000
```

---

# 🔍 What is NodePort?

NodePort exposes application through worker node port.

---

# 🔍 Service Traffic Flow

```text
Service
   ↓
Selects Pods using labels
   ↓
Routes traffic dynamically
```

---

# 🚀 STEP 22 — Deploy Flask Service

```bash
kubectl apply -f flask-service.yaml
```

---

# 🚀 STEP 23 — Install AWS Load Balancer Controller

# 🔍 Why Load Balancer Controller?

This controller automatically creates:

```text
AWS Application Load Balancer
```

from Kubernetes ingress resources.

---

# 🔹 Associate IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider \
--region ap-south-1 \
--cluster flask-cluster \
--approve
```

---

# 🔍 Why OIDC?

Allows Kubernetes service accounts to use IAM roles securely.

---

# 🚀 STEP 24 — Install Ingress Controller

# 🔍 What is Ingress?

Ingress manages external HTTP/HTTPS traffic.

---

# 🔍 Why Use Ingress?

Without ingress:

* Multiple Load Balancers needed
* Complex routing

Ingress provides:

* Central traffic routing
* Domain-based routing
* Path-based routing
* ALB integration

---

# 🚀 STEP 25 — Create Ingress

# 📄 ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: flask-ingress

spec:
  ingressClassName: alb

  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix

        backend:
          service:
            name: flask-service

            port:
              number: 5000
```

---

# 🔍 Ingress Deep Explanation

## ingressClassName: alb

Uses AWS ALB controller.

---

## backend service

Routes traffic to:

```text
flask-service
```

---

# 🔍 Traffic Flow

```text
Users
  ↓
ALB
  ↓
Ingress
  ↓
Flask Service
  ↓
Flask Pods
```

---

# 🚀 STEP 26 — Deploy Ingress

```bash
kubectl apply -f ingress.yaml
```

---

# 🚀 STEP 27 — Verify Ingress

```bash
kubectl get ingress
```

---

# 🔍 Output

```text
ADDRESS
```

Copy ALB URL.

Open in browser.

---

# ✅ Final Output

```text
🚀 Two-Tier Application on AWS EKS
Flask + HTML + MySQL + Kubernetes
```

---

# 🚨 Common Errors & Fixes

# ❌ Pods Not Running

## Check

```bash
kubectl get pods
```

---

# ❌ CrashLoopBackOff

## Check Logs

```bash
kubectl logs pod-name
```

---

# ❌ ImagePullBackOff

## Reason

Wrong Docker image name.

---

# ❌ Service Not Accessible

## Fix

```bash
kubectl get svc
```

---

# ❌ Ingress Pending

## Reason

ALB controller not installed.

---

# ❌ ALB Not Created

## Fix

Verify:

* OIDC
* IAM permissions
* Ingress controller

---

# ❌ MySQL Connection Failed

## Fix

Use service name:

```text
mysql-service
```

inside Flask application.

---

# 🎯 Important Interview Questions

# Q1. What is Kubernetes?

```text
Kubernetes is a container orchestration platform used to automate deployment, scaling, networking, and management of containerized applications.
```

---

# Q2. What is a Pod?

```text
Pod is the smallest deployable unit in Kubernetes.
```

---

# Q3. What is Deployment?

```text
Deployment manages pod creation, updates, scaling, and self-healing.
```

---

# Q4. Why use Service?

```text
Service provides stable networking and load balancing for pods.
```

---

# Q5. Why use Ingress?

```text
Ingress manages external HTTP/HTTPS access and integrates with AWS ALB.
```

---

# Q6. What is EKS?

```text
Amazon EKS is a managed Kubernetes service provided by AWS.
```

---

# Q7. Why use Docker?

```text
Docker packages applications and dependencies into portable containers.
```

---

# Q8. What is ALB?

```text
ALB distributes incoming traffic across application pods.
```

---

# 🧠 Key Learnings

Through this project I learned:

* Kubernetes Architecture
* EKS Cluster Management
* Docker Containerization
* Pod Networking
* Kubernetes Services
* Kubernetes Ingress
* AWS ALB Integration
* Persistent Storage
* Kubernetes YAML Files
* Cloud-native Application Deployment
* Scaling & Load Balancing
