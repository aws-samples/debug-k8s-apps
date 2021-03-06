+++
weight = 10


[logo]
src="images/kubecon-slide-header.png"

+++

{{% section %}}

## Section 1:
## Cluster Design

---

### Kubernetes Components

#### <img src="images/k8s-cluster-1.png" height="65%" width="65%" />


---

### How to setup a Kubernetes cluster

1. Create Controller nodes (aka the *Control Plane*)

2. Setup *etcd* and connect to Controller

3. Use *kubectl* to connect to the Control Plane

4. Install Worker nodes (aka the *Data Plane*)

5. Deploy apps and add-ons

6. ... profit?


---

### Design considerations for k8s cluster

- Control Plane: hosted or self-managed
- Data Plane: hosed or self-managed
- EC2 instance size based upon number of worker nodes
- Operating system?
- How many pods per cluster?
- Etcd co-located with master?
- Secure and backup etcd, how often?
- High availability
- Disaster recovery
- Upgrade k8s versions
- Staying up to date with k8s release cadence, CVEs and security patches
- Monitoring and logging
- One big cluster, multiple small clusters
- Blast radius

---

### Amazon EKS Architecture

#### <img src="images/k8s-cluster-2.png" width="65%", height="65%"/> 

---

<!--

---

### Amazon EKS architecture

- AWS Managed Control Plane
  - Master nodes
  - etcd cluster nodes
  - NLB for API load-balancing
- Highly available
- AWS IAM authentication
- VPC networking

---
-->
<!--

### Amazon EKS core tenets

- Platform for enterprises to run production grade workloads
- Provide a native and upstream experience (CNCF Certified)
- Provide seamless integration with AWS services
- Actively contribute to upstream project

---

--->

### Kubectl

One CLI to control your k8s cluster

<img src="images/k8s-cluster-3.png" height="65%" width="65%">

---

### Master node components

- **API-Server:** exposes APIs for  master nodes 
- **Controller Manager:** makes changes attempting to move the current state towards the desired state
- **Scheduler:** decides which pod should run on which worker node
- **etcd:** key/value data store used to store cluster state

---

### etcd design 

- Minimum 3 etcd servers 
- Spread across availability zones
- Uses RAFT protocol

---

### Worker node components

- [**kubelet**](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/): handles communication between worker and master nodes
- [**kube-proxy**](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/): handles communication between pods, nodes, and the outside world
- **container runtime** (CRI): runs containers on the node.


---

### Create EKS cluster

```
eksctl create cluster
```

---

### Create EKS cluster

```
eksctl create cluster -f resources/manifests/eks-cluster.yaml
```

---

### eksctl configuration file

```
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: debug-k8s
  region: us-east-1

nodeGroups:
  - name: nodegroup
    instanceType: m5.xlarge
    desiredCapacity: 4
    ssh:
      allow: true
      publicKeyName: arun-us-east1

cloudWatch:
  clusterLogging:
    # enable specific types of cluster control plane logs
    enableTypes: ["audit", "authenticator", "controllerManager"]
    # all supported types: "api", "audit", "authenticator", "controllerManager", "scheduler"
    # supported special values: "*" and "all"
```

{{% /section %}}
