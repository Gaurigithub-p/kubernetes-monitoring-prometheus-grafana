# Kubernetes Monitoring using Prometheus & Grafana 🚀

This project sets up **Prometheus & Grafana** on a **Minikube Kubernetes Cluster** for real-time monitoring.

---

## **📌 Prerequisites**  
Before starting, ensure you have:  
✅ **Minikube Installed** → [Install Minikube](https://minikube.sigs.k8s.io/docs/start/)  
✅ **Helm Installed** → [Install Helm](https://helm.sh/docs/intro/install/)  
✅ **kubectl Installed** → [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)  
✅ **Docker Installed** *(Optional: If using Docker driver)*  

---

## **📌 Step 1: Start Minikube**  
### **Option 1: Using Docker Driver**  
```sh
minikube start --driver=docker
```
### **Option 2: Without Docker Desktop (Using HyperKit on macOS)**  
```sh
minikube start --memory=4098 --driver=hyperkit
```
Check if Minikube is running:  
```sh
kubectl get pods -A
```

---

## **📌 Step 2: Install Helm (Windows Users - Using Chocolatey)**  
```sh
choco install kubernetes-helm
helm version
```

---

## **📌 Step 3: Install Prometheus Using Helm**  
### **1️⃣ Add the Prometheus Helm Repository**  
```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
### **2️⃣ Install Prometheus**  
```sh
helm install prometheus prometheus-community/prometheus
```
### **3️⃣ Verify Installation**  
```sh
kubectl get pods
kubectl get svc
```

---

## **📌 Step 4: Expose Prometheus as a NodePort Service**  
```sh
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
```
### **4️⃣ Get the Prometheus URL**  
```sh
minikube service prometheus-server-ext --url
```

---

## **📌 Step 5: Install Grafana Using Helm**  
### **1️⃣ Add the Grafana Helm Repository**  
```sh
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
### **2️⃣ Install Grafana**  
```sh
helm install grafana grafana/grafana
```
### **3️⃣ Get the Admin Password for Grafana**  
```sh
$encodedPassword = kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}"
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String($encodedPassword))
```
*(Default Username: **admin**)*  

---

## **📌 Step 6: Expose Grafana as a NodePort Service**  
```sh
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
```
### **6️⃣ Get the Grafana URL**  
```sh
minikube service grafana-ext --url
```
**Now login to Grafana using the above URL and credentials!** 🎉  

---

## **📌 Step 7: Configure Prometheus as a Data Source in Grafana**  
1️⃣ Go to **Grafana Home → Connections → Add Data Source**  
2️⃣ Select **Prometheus**  
3️⃣ Enter the Prometheus URL *(from Step 4)*  
4️⃣ Click **Save & Test**  

Now Grafana can access Prometheus metrics and display visualizations!  

---

## **📌 Step 8: Create a Monitoring Dashboard in Grafana**  
- Use **Pre-built Dashboard ID `3662`** for Kubernetes monitoring.  
- This provides real-time insights on **pods, CPU usage, memory, and more!**  

```sh
kubectl expose service prometheus-kube-state-metrics --type=NodePort --target-port=8080 --name=prometheus-kube-state-metrics-ext
```
```sh
minikube service prometheus-kube-state-metrics-ext --url
```

---

## **✅ Technologies Used**  
- **Kubernetes (Minikube)**
- **Helm**
- **Prometheus & Grafana**
- **Docker & Kubectl**
- **Monitoring & Observability**

