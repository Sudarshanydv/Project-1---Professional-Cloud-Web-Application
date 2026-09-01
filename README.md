# Project 1 - Professional Cloud Web Application...
## Complete Step-by-Step Guide 
        
> **What you will build (simple words):**     
> You will build a **professional cloud web application** that is:    
             
* Written as a simple web app            
* Packed using **Docker**        
* Deployed on **AWS EKS (Kubernetes)**               
* Exposed to the internet            
* Auto-scaled like a real company project
 
This guide is written **exactly how real DevOps / Cloud engineers work in companies**, step-by-step, with **commands, code, and results**.

---

## 1️⃣ What is a Professional Cloud Web Application?

### Simple Meaning

A professional cloud web application:

* Runs on cloud (AWS)
* Uses containers (Docker)
* Managed by Kubernetes
* Can handle many users
* Can scale automatically

Companies **never run apps manually**. Everything is automated and managed.

--- 

## 2️⃣ Real Industry Architecture Flow

User → Load Balancer → Kubernetes Service → Pod → Container → Web Application

---

## 3️⃣ Tools Used (Industry Standard)

| Tool       | Purpose                 |
| ---------- | ----------------------- |
| Git        | Source code management  |
| Docker     | Containerize app        |
| Docker Hub | Store image             |
| AWS EKS    | Managed Kubernetes      |
| kubectl    | Kubernetes commands     |
| YAML       | Kubernetes config files |
| HPA        | Auto scaling            |

---

## 4️⃣ Step 1: Create Project Folder

### Where to do this

📍 On your **local system terminal**

### Command

```bash
mkdir cloud-web-app
cd cloud-web-app
```

👉 **Result:** Project folder created.

---

## 5️⃣ Step 2: Create Web Application Code

### Where to add code

📍 Inside `cloud-web-app` folder

### File Name: `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>Cloud Web App</title>
</head>
<body>
  <h1>Professional Cloud Web Application</h1>
  <p>Successfully deployed using Docker and Kubernetes on AWS</p>
</body>
</html>
```

👉 **Result:** Simple web page ready.

---

## 6️⃣ Step 3: Create Dockerfile (Very Important)

### Where to add

📍 Same folder as `index.html`

### File Name: `Dockerfile`

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

### Why this is used

* Nginx serves web page
* Docker makes app portable

---

## 7️⃣ Step 4: Build Docker Image

### Command

```bash
docker build -t cloudwebapp:v1 .
```

### Check Image

```bash
docker images
```

👉 **Result:** Image created successfully.

---

## 8️⃣ Step 5: Test Application Locally

```bash
docker run -d -p 8080:80 cloudwebapp:v1
```

### Open Browser

```
http://localhost:8080
```

👉 **Result:** Web page visible locally.

---

## 9️⃣ Step 6: Push Docker Image to Docker Hub

### Login

```bash
docker login
```

### Tag Image

```bash
docker tag cloudwebapp:v1 yourdockerhub/cloudwebapp:v1
```

### Push Image

```bash
docker push yourdockerhub/cloudwebapp:v1
```

👉 **Result:** Image stored in Docker Hub.

---

## 🔟 Step 7: Create AWS EKS Cluster

```bash
eksctl create cluster --name cloud-cluster --region ap-south-1
```

👉 **Result:** Kubernetes cluster created.

---

## 1️⃣1️⃣ Step 8: Connect kubectl to Cluster

```bash
aws eks update-kubeconfig --region ap-south-1 --name cloud-cluster
kubectl get nodes
```

👉 **Result:** Worker nodes visible.

---

## 1️⃣2️⃣ Step 9: Create Deployment

### Where to add

📍 Create file `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cloudwebapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cloudwebapp
  template:
    metadata:
      labels:
        app: cloudwebapp
    spec:
      containers:
      - name: cloudwebapp
        image: yourdockerhub/cloudwebapp:v1
        ports:
        - containerPort: 80
```

### Apply Deployment

```bash
kubectl apply -f deployment.yaml
kubectl get pods
```

👉 **Result:** 2 pods running.

---

## 1️⃣3️⃣ Step 10: Expose Application (Service)

### File Name: `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: cloudwebapp-service
spec:
  type: LoadBalancer
  selector:
    app: cloudwebapp
  ports:
  - port: 80
    targetPort: 80
```

### Apply Service

```bash
kubectl apply -f service.yaml
kubectl get svc
```

👉 **Result:** External IP generated.

---

## 1️⃣4️⃣ Step 11: Access Application

Open browser:

```
http://<EXTERNAL-IP>
```

✔️ Application is live on cloud.

---

## 1️⃣5️⃣ Step 12: Enable Auto Scaling (HPA)

```bash
kubectl autoscale deployment cloudwebapp-deployment \
--cpu-percent=50 --min=2 --max=6
```

Check:

```bash
kubectl get hpa
```

👉 **Result:** Pods scale automatically.

---

## 1️⃣6️⃣ Final Result (What You Built)

✔️ Cloud-based web application
✔️ Dockerized application
✔️ Kubernetes-managed deployment
✔️ Internet exposed using LoadBalancer
✔️ Auto-scaling enabled

---

## 1️⃣7️⃣ Resume Project Description

> "Built and deployed a professional cloud web application using Docker and Kubernetes on AWS EKS, implemented LoadBalancer services and horizontal pod autoscaling for scalable and highly available infrastructure."

---

## 1️⃣8️⃣ Interview Explanation (Simple)

> "I containerized a web application using Docker, deployed it on AWS EKS using Kubernetes deployments and services, exposed it via a LoadBalancer, and enabled autoscaling to handle traffic automatically."

---

## 1️⃣9️⃣ Next Enhancements (Advanced)

* CI/CD with GitHub Actions
* Ingress controller
* Custom domain
* Monitoring with Prometheus
* Helm charts

👉 Tell me what you want next 👍
