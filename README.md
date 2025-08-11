# Kubernetes Minikube Project - Nginx Deployment

## 🎯 Objective
Deploy an Nginx web application on Kubernetes using Minikube, expose it via a NodePort service, scale it, and access it in the browser.

---

## 📌 Steps Performed
```bash
#1. Start Minikube

minikube start --driver=docker

#2. Create Deployment
File: deployment.yaml


apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx:latest
        ports:
        - containerPort: 80
Apply:


kubectl apply -f deployment.yaml
kubectl get pods


#3.Create Service
File: service.yaml


apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
Apply:


kubectl apply -f service.yaml
kubectl get svc


#4. Access Application

minikube service myapp-service --url

http://192.168.49.2:30080
Open in browser to see "Welcome to Nginx" page.


#5. Scaling Deployment

kubectl scale deployment myapp-deployment --replicas=4
kubectl get pods
kubectl rollout status deployment/myapp-deployment


#6. Rolling Update

kubectl set image deployment/myapp-deployment myapp=nginx:1.21.6
kubectl rollout status deployment/myapp-deployment


#7. Rollback (if needed)


kubectl rollout undo deployment/myapp-deployment

