# Kubernetes Learning Journey

Welcome to my Kubernetes learning repository! This project documents my experiments with container orchestration, covering stateless applications, stateful databases, persistent storage, and frontend-backend integration.

## 📂 Repository Structure

The project is divided into three main components:

| Directory | Description | Key Concepts |
| :--- | :--- | :--- |
| **`db-demo-app`** | Node.js app with MongoDB connection | Persistent Volumes, PVCs, ConfigMaps, Sidecars |
| **`express-demo`** | Lightweight Node.js server | Deployments, Services, Stateless Apps |
| **`my-app`** | React Frontend application | Containerization, Static Site Serving |

---

## 🚀 Projects & Experiments

### 1. Database Demo App (`/db-demo-app`)
This is the core learning module focused on stateful applications. It demonstrates how to connect a Node.js application to MongoDB within a Kubernetes cluster.

**Key Features:**
* **REST API:** Endpoints to add and retrieve emails (`GET /`, `POST /add-email`, `GET /emails`).
* **Configuration:** Uses `ConfigMap` to decouple database connection strings (`MONGO_HOST`, `MONGO_PORT`) from the application code.
* **Persistence:** Implements `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC) to ensure database data survives pod restarts.
    * *Storage Type:* `hostPath` (Maps `/data/` on the host machine to the container).

**Evolution of Deployment:**
* *Approach A (Sidecar):* Initially deployed the Node.js app and MongoDB in the same Pod for tight coupling.
* *Approach B (Decoupled):* Separated MongoDB into its own Deployment and Service (`service-mongodb`), allowing the Node.js app to connect via DNS.

### 2. Express Demo (`/express-demo`)
A foundational "Hello World" style application to understand basic Kubernetes objects.

* **Deployment:** Runs 2 replicas of a simple Node.js server.
* **Function:** Serves a simple text response to verify the cluster is running.

### 3. React Frontend (`/my-app`)
A standard Create React App project intended to serve as the user interface.

* **Stack:** React 19, Web Vitals, Jest.

---

## 🛠️ Setup & Installation

To run these experiments on a local cluster (like Minikube or Docker Desktop), follow these steps:

### Prerequisites
* Kubernetes Cluster (Minikube, Kind, or Docker Desktop)
* `kubectl` CLI installed

### Step 1: Deploy Storage & Config
First, set up the persistent volume and configuration so the database has a place to store data.

```bash
# Apply Storage definitions
kubectl apply -f db-demo-app/host-pv.yaml
kubectl apply -f db-demo-app/host-pvc.yaml

# Apply Configuration
kubectl apply -f db-demo-app/mongo-config.yaml
```


### Step 2: Deploy the Database
Deploy MongoDB using the persistent storage created above.

```bash
kubectl apply -f db-demo-app/deployment-service-mongo.yaml
```

### Step 3: Deploy the Applications
Deploy the Node.js backend and expose it.

```bash
# Deploy the DB Demo App
kubectl apply -f db-demo-app/deployment-service-db-demo-app1.yaml

# (Optional) Deploy the simple Express app
kubectl apply -f express-demo/deployment-node-app.yaml
```

Step 4: Access the App
The db-demo-app service is configured as a LoadBalancer on port 8080.

URL: ``` http://localhost:8080 ```
API: ``` http://localhost:8080/emails ```