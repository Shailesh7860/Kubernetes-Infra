# Kubernetes Infrastructure for Jellyfin

## What the project does
This project is a Kubernetes learning lab that deploys a Jellyfin media server on a local Kind cluster. It is designed to practice core Kubernetes concepts such as deployments, services, persistent storage, and local cluster networking in a simple real-world setup.

## Why you built it
I built this project to learn and practice Kubernetes hands-on. The goal is to become more comfortable with writing YAML manifests, creating local clusters, applying workloads, and understanding how containers are exposed and managed inside Kubernetes.

## Architecture / diagram
The setup is intentionally simple and focused on learning Kubernetes fundamentals in a local environment.

```text
+-------------------+
| Host machine     |
|                  |
| kind cluster     |
|  +------------+  |
|  | Deployment |  |
|  | jellyfin   |  |
|  | 3 replicas |  |
|  +------------+  |
|       |           |
|       v           |
|  +------------+  |
|  | Service    |  |
|  | NodePort   |  |
|  +------------+  |
|       |           |
|       v           |
|  +------------+  |
|  | PVC        |  |
|  | config     |  |
|  +------------+  |
+-------------------+
```

The application runs inside the cluster, and the service exposes port 8096 through NodePort 30007. The bound PVC keeps configuration data persistent across pod restarts.

## Technologies used
- Kubernetes
- Kind
- YAML manifests
- Docker
- Jellyfin
- PersistentVolumeClaim for storage

## How you deployed it
This project is used as a practice setup for learning Kubernetes. The deployment flow is:

1. Create a local Kind cluster using the configuration in `kind/cluster.yaml`.
2. Apply the PVC manifest from `k8s/pvc-config.yaml`.
3. Deploy the Jellyfin app using `k8s/deployment.yaml`.
4. Expose the app with a NodePort service from `k8s/service.yaml`.
5. Validate the workload and networking with `kubectl` commands.

Example flow:

```bash
kind create cluster --config kind/cluster.yaml
kubectl apply -f k8s/pvc-config.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get pods
kubectl get svc
```

## Problems you encountered
- Learning how to correctly expose an application from a local cluster to the host machine.
- Understanding why stateful applications need persistent storage.
- Debugging the relationship between container ports, service ports, and NodePort mappings.

## How you solved them
- Used a `NodePort` service to practice exposing an application outside the cluster.
- Added a PVC for `/config` so Jellyfin data persists and can be studied in a real-world way.
- Configured Kind port mapping in `kind/cluster.yaml` to forward traffic from the host to the Kubernetes service.

## Screenshots
No screenshots captured yet for this project, but this is the expected local access pattern:

- Application URL: http://localhost:30007
- Jellyfin dashboard runs on port 8096 inside the cluster and is exposed via NodePort 30007.

## What you learned
- How to define and apply Kubernetes manifests for deployments, services, and volumes.
- How Kind is used to create a local Kubernetes environment.
- Why persistent storage is essential for stateful applications.
- How networking and service exposure work in Kubernetes.
- How to practice Kubernetes by changing YAML, applying it, and validating the actual cluster behavior.

## Current status: In progress / learning project
This project is an active Kubernetes practice environment. It is being used to learn and improve practical skills with local cluster setup, workload deployment, storage, and service exposure.
