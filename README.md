# 🧰 Kubernetes Practice Tasks — DEPI Training

This repository contains a set of Kubernetes practice tasks completed as part of the **DEPI DevOps Training** program.  
The tasks cover various fundamental Kubernetes concepts such as **Deployments**, **Volumes**, **Secrets**, **ConfigMaps**, **Network Policies**, and **Taints & Tolerations**.

---

## 📝 Task 1: Deploy a Simple Web Frontend (Nginx)

**Namespace:** `fe`  
**Objective:** Deploy a simple Nginx web application using an `emptyDir` volume and expose it externally.

- Created a namespace `fe`.
- Deployed an Nginx Deployment with **2 replicas**.
- Used an `emptyDir` volume mounted to `/usr/share/nginx/html`.
- Exposed the application using a **NodePort Service** on port **80**.

> 🧠 Since the volume was intentionally left empty, the Nginx server returned a `502 Bad Gateway` response, which is expected behavior.

---

## 📝 Task 2: Deploy MongoDB with Persistent Storage

**Namespace:** `mongo-db`  
**Objective:** Deploy a MongoDB database with persistent storage, credentials, and internal service exposure.

- Created a **Secret** `mongodb-secret` to store:
  - `MONGO_INITDB_ROOT_USERNAME=admin`
  - `MONGO_INITDB_ROOT_PASSWORD=admin123`
- Created a **PersistentVolume** and **PersistentVolumeClaim** named `mongodb-pvc` to store data at `/data/db`.
- Deployed MongoDB using `mongo:latest` image.
- Created a **ClusterIP Service** `mongodb-service` on port **27017** for internal access.

---

## 📝 Task 3: Deploy Mongo Express Dashboard

**Namespace:** `mongo-express`  
**Objective:** Deploy the Mongo Express web interface to manage MongoDB.

- Created a **ConfigMap** `mongo-express-config` for environment variables.
- Created a **Secret** `mongo-express-secret` to store admin credentials.
- Deployed `mongo-express:latest` image as a Deployment with **1 replica**.
- Exposed the service using a **NodePort Service** `mongo-express-service` on port **8081**.
- Connected Mongo Express to MongoDB using the internal ClusterIP service.

---

## 📝 Task 4: Apply Network Policy for Namespace Isolation

**Objective:** Restrict MongoDB access to only the `mongo-express` namespace.

- Labeled the `mongo-express` namespace:
  ```bash
  kubectl label namespace mongo-express access=mongo-express
  ```
- Created a **NetworkPolicy** in the `mongo-db` namespace to only allow traffic from namespaces with the label `access=mongo-express` on port `27017`.

✅ Result: Only Pods in `mongo-express` can access MongoDB. All other traffic is blocked.

---

## 📝 Task 5: Use Taints and Tolerations for MongoDB Scheduling

**Objective:** Control scheduling of MongoDB Pod using taints and tolerations.

- Tainted the `node01` node:
  ```bash
  kubectl taint nodes node01 db=NoSchedule:NoSchedule
  ```
- Added a **Toleration** in the `mongodb-deployment` to allow the Pod to be scheduled on the tainted node:
  ```yaml
  tolerations:
    - key: "db"
      operator: "Equal"
      value: "NoSchedule"
      effect: "NoSchedule"
  ```

✅ Result: The MongoDB Pod is scheduled on the tainted worker node (`node01`) while other Pods without the toleration cannot be scheduled there.

---

## 🧠 What I Practiced

- Namespaces & resource isolation
- Deployments and Services
- Volumes & Persistent storage
- Secrets and ConfigMaps
- Port forwarding & external access
- Network Policies for traffic control
- Taints & Tolerations for node scheduling

---

## ✨ Author

**Muhammad Essam**  
DEPI DevOps Trainee 🚀
