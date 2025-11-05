# 🧩 DVWA Kubernetes Lab

This project demonstrates the deployment of the **Damn Vulnerable Web Application (DVWA)** on a **local Kubernetes cluster** using **Minikube**.
DVWA is a deliberately insecure web application designed for learning and practicing web security testing.

---

## 🚀 Project Overview

The setup uses **Kubernetes** to deploy two core components:

* **MySQL Database** – stores user and application data.
* **DVWA Web Application** – provides vulnerable web interfaces for testing attacks.

The goal is to simulate a real-world containerized environment and explore how common web vulnerabilities behave within a Kubernetes setup.

---

## ⚙️ Deployment Steps

### 🧰 Tools Used

* **Minikube** – local Kubernetes cluster
* **Docker** – container runtime
* **kubectl** – cluster management
* **DVWA & MySQL images** – from Docker Hub

### 🪄 Setup Commands

```bash
# Start local Kubernetes cluster
minikube start --driver=docker

# Apply manifests
kubectl apply -f mysql-deployment.yaml
kubectl apply -f dvwa-configmap.yaml
kubectl apply -f dvwa-deployment.yaml

# Check pod and service status
kubectl get pods
kubectl get svc

# Get the Minikube IP and access DVWA
minikube ip
# → Open in browser: http://<minikube-ip>:30080/
```

---

## 🧠 Attacks Demonstrated

### 1️⃣ SQL Injection

* **Description:** Injecting malicious SQL queries to bypass authentication or retrieve data.
* **Example Payload:** `' OR '1'='1`
* **Result:** Displays all user data due to improper query sanitization.

### 2️⃣ Command Injection

* **Description:** Running system commands through unsanitized input fields.
* **Example Payload:** `127.0.0.1; ls -la`
* **Result:** Executes OS commands on the server.

### 3️⃣ Stored XSS (Cross-Site Scripting)

* **Description:** Injecting malicious scripts that run in other users’ browsers.
* **Example Payload:** `<script>alert('XSS')</script>`
* **Result:** Displays an alert popup, proving script execution.

Screenshots and HTML evidence of each attack are included in the `evidence/` folder.

---

## 📂 Repository Structure

```
manifests/         → Kubernetes YAML files for DVWA and MySQL
evidence/           → Screenshots, logs, and HTML snapshots
commands_used.txt   → All shell commands executed
README.md           → Project documentation
```

---

## 🧹 Cleanup

After testing, delete all resources and stop the cluster:

```bash
kubectl delete -f dvwa-deployment.yaml
kubectl delete -f dvwa-configmap.yaml
kubectl delete -f mysql-deployment.yaml
minikube stop
minikube delete
```

---

## ⚠️ Disclaimer

This project is for **educational and cybersecurity training purposes only**.
All testing was performed in a **local, isolated environment**.
Do **not** deploy DVWA or similar vulnerable apps on public servers.

---
