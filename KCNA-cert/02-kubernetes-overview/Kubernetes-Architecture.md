# Kubernetes Architecture

Kubernetes is a **container orchestration system** that automates the deployment, scaling, and management of containerized applications.

Its architecture is divided into two main parts:

- **Control Plane** → the brain of the cluster  
- **Worker Nodes** → the machines that run your applications  

---

## 1. Cluster Overview

The Kubernetes cluster consists of a **Control Plane** and multiple **Worker Nodes**.

                   +-----------------------------+
                   |        Control Plane        |
                   |-----------------------------|
                   | - API Server                |
                   | - etcd                      |
                   | - Scheduler                 |
                   | - Controller Manager        |
                   +-------------+---------------+
                                 |
              +------------------+------------------+
              |                                     |
     +--------------------+              +--------------------+
     |    Worker Node 1   |              |    Worker Node 2   |
     |--------------------|              |--------------------|
     | - Kubelet          |              | - Kubelet          |
     | - Kube-proxy       |              | - Kube-proxy       |
     | - Container runtime|              | - Container runtime|
     | - Pods             |              | - Pods             |
     +--------------------+              +--------------------+



The **Control Plane** makes decisions and maintains the cluster state.  
The **Worker Nodes** execute the workloads and run containers inside Pods.

---

## 2. Control Plane Components

The Control Plane is responsible for the global decisions of the cluster.

| Component | Description |
|------------|-------------|
| **API Server (`kube-apiserver`)** | The main management component. It exposes the Kubernetes API and is the entry point for all administrative tasks. |
| **etcd** | A distributed key-value store that holds all cluster data, configuration, and state. |
| **Scheduler** | Decides on which node each new Pod should run, based on resource usage and policies. |
| **Controller Manager** | Runs background processes (controllers) that maintain the desired cluster state. Examples: Node controller, ReplicaSet controller. |
| **Cloud Controller Manager** | Manages integration with cloud providers (AWS, Azure, GCP). |

---

## 3. Worker Node Components

Worker Nodes host the Pods that run your applications.  
Each node includes the following key components:

| Component | Description |
|------------|-------------|
| **kubelet** | Communicates with the Control Plane and ensures containers are running as expected. |
| **kube-proxy** | Manages network routing and load balancing between Pods and Services. |
| **Container Runtime** | The software that actually runs containers (e.g., `containerd`, `CRI-O`, or Docker). |
| **Pods** | The smallest deployable unit — a group of containers sharing storage and network resources. |

---

## 4. How Components Interact

The communication flow in a Kubernetes cluster:

1. kubectl → API Server → etcd (stores state)
↓
2. Scheduler assigns node
↓
3. kubelet on that node creates Pod
↓
4. Container Runtime runs containers
↓
5. kube-proxy handles networking


---

## 5. Example Workflow

When a user runs the following command:

```bash
kubectl run nginx --image=nginx

Kubernetes performs these steps internally:

kubectl sends the request to the API Server.

The API Server stores the desired state (Pod specification) in etcd.

The Scheduler detects an unscheduled Pod and assigns it to a Node.

The kubelet on that Node pulls the nginx image and runs the container using the container runtime.

The kube-proxy sets up networking rules to make the Pod accessible.

