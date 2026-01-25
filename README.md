# Professional Cloud Web Application – Complete Step‑by‑Step Guide (Industry Level)

> **Goal (What you will build):**
> You will take a simple web application, **containerize it using Docker**, **deploy it on AWS using EKS (Kubernetes)**, **expose it to the internet**, and **enable auto‑scaling** like a real industry project.

This guide is written in **very simple, clear English**, step‑by‑step, exactly how **real DevOps / Cloud engineers work in companies**.

---

## 1️⃣ What is a Cloud Web Application? (Big Picture)

A **Cloud Web Application** is an application that:

* Runs on cloud servers (AWS)
* Is packaged using containers (Docker)
* Is managed using orchestration tools (Kubernetes)
* Can scale automatically when users increase
* Is highly available and reliable

### Industry Flow (High Level)

User → Load Balancer → Kubernetes Service → Pod → Container → Application

---

## 2️⃣ Step‑by‑Step Architecture (Industry Standard)

### Tools Used

| Tool         | Why we use it              |
| ------------ | -------------------------- |
| Git          | Store source code          |
| Docker       | Create application image   |
| Docker Hub   | Store Docker image         |
| AWS EKS      | Managed Kubernetes cluster |
| Kubernetes   | Deploy & manage containers |
| LoadBalancer | Expose app to internet     |
| HPA          | Auto scale application     |

---

## 3️⃣ Step 1: Create a Simple Web Application

### Example: Simple HTML Web App

**index.html**

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Cloud App</title>
</head>
<body>
  <h1>Welcome to My Cloud Web Application</h1>
  <p>Deployed using Docker and Kubernetes on AWS EKS</p>
</body>
</html>
```

👉 **Result:** When opened in browser, this page shows a welcome message.

---

## 4️⃣ Step 2: Create Dockerfile (Containerization)

### What is Docker?

Docker packages your application **with everything it needs** (code, libraries, runtime).

### Dockerfile (Industry Level)

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

### Explanation (Line by Line)

* `FROM nginx:alpine` → Lightweight web server
* `COPY` → Copies your app file into container
* `EXPOSE 80` → Application runs on port 80

---

## 5️⃣ Step 3: Build Docker Image

```bash
docker build -t mycloudapp:v1 .
```

### Check Image

```bash
docker images
```

👉 **Result:** You will see `mycloudapp:v1` in the list.

---

## 6️⃣ Step 4: Run Container Locally (Testing)

```bash
docker run -d -p 8080:80 mycloudapp:v1
```

### Open Browser

```
http://localhost:8080
```

👉 **Result:** Your web page opens successfully.

✔️ This confirms Docker image works correctly.

---

## 7️⃣ Step 5: Push Image to Docker Hub

### Login

```bash
docker login
```

### Tag Image

```bash
docker tag mycloudapp:v1 yourusername/mycloudapp:v1
```

### Push

```bash
docker push yourusername/mycloudapp:v1
```

👉 **Result:** Image is stored in Docker Hub (cloud image repository).

---

## 8️⃣ Step 6: Create AWS EKS Cluster

### Why EKS?

* AWS manages Kubernetes control plane
* Secure, scalable, production ready

### Create Cluster (Using eksctl)

```bash
eksctl create cluster --name mycluster --region ap-south-1
```

👉 **Result:** Kubernetes cluster created in AWS.

---

## 9️⃣ Step 7: Connect kubectl to EKS

```bash
aws eks update-kubeconfig --region ap-south-1 --name mycluster
```

### Test Connection

```bash
kubectl get nodes
```

👉 **Result:** Worker nodes will be listed.

---

## 🔟 Step 8: Kubernetes Deployment (Run Containers)

### deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cloudapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cloudapp
  template:
    metadata:
      labels:
        app: cloudapp
    spec:
      containers:
      - name: cloudapp
        image: yourusername/mycloudapp:v1
        ports:
        - containerPort: 80
```

### Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

### Check Pods

```bash
kubectl get pods
```

👉 **Result:** 2 running pods.

---

## 1️⃣1️⃣ Step 9: Expose App using Service (LoadBalancer)

### service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: cloudapp-service
spec:
  type: LoadBalancer
  selector:
    app: cloudapp
  ports:
  - port: 80
    targetPort: 80
```

### Apply Service

```bash
kubectl apply -f service.yaml
```

### Get External IP

```bash
kubectl get svc
```

👉 **Result:** External LoadBalancer URL.

### Open Browser

```
http://<EXTERNAL-IP>
```

✔️ Application is now live on the internet.

---

## 1️⃣2️⃣ Step 10: Enable Auto Scaling (HPA)

### Why HPA?

Automatically increases/decreases pods based on CPU load.

### Enable Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Create HPA

```bash
kubectl autoscale deployment cloudapp-deployment --cpu-percent=50 --min=2 --max=5
```

### Check HPA

```bash
kubectl get hpa
```

👉 **Result:** Pods auto‑scale when traffic increases.

---

## 1️⃣3️⃣ Final Output (What You Achieved)

✔️ Web application running on AWS cloud
✔️ Dockerized application
✔️ Kubernetes managed deployment
✔️ LoadBalancer exposed to internet
✔️ Auto‑scaling enabled

---

## 1️⃣4️⃣ How to Explain This in Interview (Industry Answer)

> "I containerized the application using Docker, pushed the image to Docker Hub, deployed it on AWS EKS using Kubernetes Deployments and Services, exposed it through a LoadBalancer, and implemented Horizontal Pod Autoscaling to handle variable traffic efficiently."

---

## 1️⃣5️⃣ Next Up (If You Want)

* Architecture Diagram (Image)
* Convert this into **PDF**
* Add **CI/CD (GitHub Actions)**
* Add **Ingress + Domain**
* Add **Monitoring (Prometheus, Grafana)**

Just tell me 👍
