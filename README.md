# Kubernetes Basic Resources

A brief summary of the features and purpose of the resource covered by each YAML file in this repository.

## Cluster

| File | Resource | Description |
|------|----------|-------------|
| [cluster.yaml](cluster.yaml) | `ClusterConfig` (eksctl) | eksctl config defining the EKS cluster and node groups. Create with `eksctl create cluster -f cluster.yaml` |
| [namespace.yaml](namespace.yaml) | `Namespace` | Logical partition of the cluster. Prevents resource name collisions; scope for RBAC and quotas |

## Workloads

| File | Resource | Description |
|------|----------|-------------|
| [pod.yaml](pod.yaml) | `Pod` | Smallest deployable unit. One or more containers sharing network and storage. Does not recover on its own if it dies |
| [deployment.yaml](deployment.yaml) | `Deployment` | For stateless apps. Maintains Pod count via ReplicaSet; supports rolling updates and rollbacks |
| [statefulset.yaml](statefulset.yaml) | `StatefulSet` + Headless `Service` | For stateful apps (e.g. databases). Stable per-Pod names (`app-0`, `app-1`) and volumes; ordered creation/deletion |
| [daemonset.yaml](daemonset.yaml) | `DaemonSet` | Runs one Pod on every (or selected) node. Used for log collectors and monitoring agents |
| [job.yaml](job.yaml) | `Job` | Runs a task once to completion. Retries up to `backoffLimit` times on failure |
| [cronjob.yaml](cronjob.yaml) | `CronJob` | Creates Jobs periodically on a cron schedule. Used for backups and batch jobs |

## Networking

| File | Resource | Description |
|------|----------|-------------|
| [service.yaml](service.yaml) | `Service` | Provides a stable IP/DNS and load balancing for a set of Pods. Types: `ClusterIP` (internal), `NodePort`, `LoadBalancer` (external) |
| [ingress.yaml](ingress.yaml) | `Ingress` | Routes HTTP/HTTPS requests to Services by host and path. Requires an Ingress Controller |
| [networkpolicy.yaml](networkpolicy.yaml) | `NetworkPolicy` | Rules allowing/blocking traffic between Pods (firewall). Requires CNI support |

## Configuration

| File | Resource | Description |
|------|----------|-------------|
| [configmap.yaml](configmap.yaml) | `ConfigMap` | Stores general config as key-value pairs. Injected into Pods as env vars or files |
| [secret.yaml](secret.yaml) | `Secret` | Stores sensitive data such as passwords and tokens. Only base64-encoded, not encrypted, so access control is required |

## Storage

| File | Resource | Description |
|------|----------|-------------|
| [storageclass.yaml](storageclass.yaml) | `StorageClass` | Defines the volume type (e.g. EBS gp3) and dynamic provisioning method |
| [pvc.yaml](pvc.yaml) | `PersistentVolumeClaim` | Requests the storage size and access mode a Pod needs. A PV is created automatically via the StorageClass |

## Scalability & Availability

| File | Resource | Description |
|------|----------|-------------|
| [hpa.yaml](hpa.yaml) | `HorizontalPodAutoscaler` | Automatically scales Pod count based on metrics like CPU and memory. Requires metrics-server |
| [pdb.yaml](pdb.yaml) | `PodDisruptionBudget` | Guarantees a minimum number of available Pods during voluntary disruptions such as node drains |

## Security

| File | Resource | Description |
|------|----------|-------------|
| [rbac.yaml](rbac.yaml) | `ServiceAccount`, `Role`, `RoleBinding` | ServiceAccount is a Pod's identity, Role is a set of permissions within a namespace, RoleBinding links the two |

## Common Commands

```bash
kubectl apply -f <file>.yaml          # Create/update
kubectl get <resource> -n <ns>        # List
kubectl describe <resource> <name>    # Show details and events
kubectl delete -f <file>.yaml         # Delete
```
