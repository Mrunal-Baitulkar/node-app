 ## 🌐 Node.js Web App Deployment On DigitalOcean Kubernetes

This repository contains a web application served using Node.js and deployed on DigitalOcean Kubernetes Service. The app is containerized using Docker and supports horizontal pod autoscaling and load balancing.


🛠️ Prerequisites
Before proceeding, ensure the following tools are installed on your local machine:


## 📁 Project Structure

```
├── Dockerfile
├── README.md
├── app.js
├── deployment.yaml  #deployment manifest file for web app
├── hpa.yaml               #Horizontal Pod Autoscaler
├── package.json         #nodejs dependencies
├── public
│   ├── digitalocean-logo.png
│   └── index.html      #html file content
└── service.yaml    #Kubernetes LoadBalancer Service manifest
```

## Step 1: Clone the Repository
```

git clone https://github.com/Mrunal-Baitulkar/node-app.git
cd node-app
```


## 🐳 Step 2: Build and Push Docker Image
```
Replace <your-dockerhub-username> with your Docker Hub username.
docker build -t <dockerhub-username>/node-app:latest --load .

docker login

docker push <dockerhub-username>/node-app:latest
```

      
## Step 3: ☁️ Deploy to Kubernetes (DigitalOcean)
Create cluster using UI or using doctl CLI
1. Authenticate doctl
```
doctl auth init
```
2. Create Kubernetes Cluster
```
doctl kubernetes cluster create web-app-cluster \
  --region nyc1 \
  --node-pool “name=web-pool;size=s-2vcpu-2gb;count=2;auto-scale=true;min-nodes=2;max-nodes=3”
```

## Step 4: Configure kubectl Access

```
doctl kubernetes cluster list
```

1. Save kubeconfig locally
```
doctl kubernetes cluster kubeconfig save <cluster-ID> #from the above step
# or
doctl kubernetes cluster kubeconfig save k8s-deploy

```

2. Verify cluster access
Now, you can successfully authenticate to the Kube API server
```
kubectl get nodes -o wide # Displays the no. of nodes in the cluster
Kubectl get pods -A #Displays all the pods created in the cluster

```

## 🚀 Step 5: Deploy Your Application
1. Create a namespace
```
Kubectl create namespace -n node-app
```

2. Apply the deployment and service file
```
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

3. Check service status and external IP:
```
kubectl get svc
```

## Once an external IP is assigned, access your application using the external IP .

Step 6: 📊 Enable Horizontal Pod Autoscaler
Install Metrics Server
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/
latest/download/components.yaml
```
Apply HPA manifest
```
kubectl apply -f hpa.yaml
```
Verify HPA status
```
kubectl get hpa
```

