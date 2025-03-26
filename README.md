#  Kubernetes Multi-Service Deployment - Jagadesh

This project demonstrates deploying a **Flask web application** on **Kubernetes** using different service types: **ClusterIP** and **LoadBalancer**.

## Features

✅ Runs inside Kubernetes  
✅ Exposed using ClusterIP and LoadBalancer Services  
✅ Uses Dockerized Flask App  

##  Deployment YAML (`deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloapp-deployment
  labels:
    app: helloapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: helloapp
  template:
    metadata:
      labels:
        app: helloapp
    spec:
      containers:
        - name: helloapp
          image: jagadesh999/helloapp:latest
          ports:
            - containerPort: 5000
```

---

##  ClusterIP Service (`clusterip-service.yaml`)

This Service is **internal-only**, exposing the app **inside the cluster**.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: helloapp-clusterip
spec:
  selector:
    app: helloapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: ClusterIP
```

 **Access:** Inside the cluster only.  
```sh
kubectl exec -it <pod-name> -- curl http://helloapp-clusterip
```

---

##  LoadBalancer Service (`loadbalancer-service.yaml`)

This Service exposes the app **externally** using a LoadBalancer.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: helloapp-loadbalancer
spec:
  selector:
    app: helloapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: LoadBalancer
```

 **Access:** External Load Balancer will provide a public IP.  
```sh
kubectl get svc helloapp-loadbalancer
```

---

## 🚀 Deploy Everything to Kubernetes

```sh
kubectl apply -f deployment.yaml
kubectl apply -f clusterip-service.yaml
kubectl apply -f loadbalancer-service.yaml
```

✅ **Check Pods & Services**  
```sh
kubectl get pods
kubectl get svc
```

---

 **If you like this project, give it a star on GitHub!** 🌟

