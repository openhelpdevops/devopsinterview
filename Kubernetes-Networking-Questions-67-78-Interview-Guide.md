# Kubernetes Networking & Service Discovery (67–78) Interview Guide

## 67. How does the Kubernetes networking model ensure every Pod gets a unique IP?

### Answer
Kubernetes assigns every Pod its own IP address using a CNI plugin (Calico, Flannel, Cilium).

Key Rules:
- Every Pod gets a unique IP.
- Pods communicate directly without NAT.
- Pods on different nodes can communicate.

### Architecture

```mermaid
flowchart LR
    Pod1["Pod 10.244.1.10"]
    Pod2["Pod 10.244.2.20"]
    CNI["Calico / Cilium"]

    Pod1 --> CNI --> Pod2
```

### Commands

```bash
kubectl get pods -o wide
```

---

## 68. What is a Kubernetes Service, and why is it necessary for Pod communication?

### Answer

Pods are temporary and their IPs change.

A Service provides:
- Stable IP
- Stable DNS Name
- Load Balancing

```mermaid
flowchart LR
    Client --> Service
    Service --> Pod1
    Service --> Pod2
    Service --> Pod3
```

### Sample YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: frontend
```

---

## 69. Explain the ClusterIP service type and its scope

### Answer

ClusterIP is the default service type.

Used for internal communication only.

```mermaid
flowchart LR
    PodA --> ClusterIP
    ClusterIP --> PodB
```

### Example

```yaml
spec:
  type: ClusterIP
```

Access:

```bash
curl http://service-name
```

---

## 70. Explain the NodePort service type and how it exposes traffic

### Answer

NodePort exposes an application using a port on every worker node.

Range:

```text
30000-32767
```

### Architecture

```mermaid
flowchart LR
    User --> NodeIP
    NodeIP --> NodePort
    NodePort --> Pod
```

### YAML

```yaml
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30080
```

---

## 71. Explain the LoadBalancer service type in cloud environments

### Answer

LoadBalancer creates a cloud load balancer.

Examples:
- AWS ELB
- Azure LB
- GCP LB
- MetalLB (On-Prem)

### Architecture

```mermaid
flowchart LR
    Internet --> LoadBalancer
    LoadBalancer --> Service
    Service --> Pods
```

### YAML

```yaml
spec:
  type: LoadBalancer
```

---

## 72. What is a Headless Service (ClusterIP: None) and when is it used?

### Answer

Headless Service does not create a ClusterIP.

Used by:
- StatefulSets
- Databases
- Kafka

### Architecture

```mermaid
flowchart LR
    Client --> mysql-0
    Client --> mysql-1
    Client --> mysql-2
```

### YAML

```yaml
spec:
  clusterIP: None
```

---

## 73. What is an ExternalName service type?

### Answer

Maps a Kubernetes Service to an external DNS name.

### Architecture

```mermaid
flowchart LR
    App --> ExternalName
    ExternalName --> database.company.com
```

### YAML

```yaml
spec:
  type: ExternalName
  externalName: database.company.com
```

---

## 74. How does DNS resolution (CoreDNS) work internally for K8s Services?

### Answer

CoreDNS provides service discovery.

Flow:

```mermaid
sequenceDiagram
    Pod->>CoreDNS: frontend.default.svc.cluster.local
    CoreDNS-->>Pod: 10.96.20.15
```

### Commands

```bash
kubectl get svc
kubectl get pods -n kube-system
```

Test DNS:

```bash
nslookup kubernetes.default
```

---

## 75. What is an Ingress and how does it differ from a LoadBalancer Service?

### Answer

Ingress provides Layer-7 routing.

LoadBalancer provides Layer-4 exposure.

### Architecture

```mermaid
flowchart LR
    User --> Ingress
    Ingress --> Service1
    Ingress --> Service2
```

### Example

```yaml
kind: Ingress
```

### Comparison

| Ingress | LoadBalancer |
|----------|-------------|
| Layer 7 | Layer 4 |
| Host Routing | No Host Routing |
| Path Routing | No Path Routing |
| TLS Termination | Limited |

---

## 76. What is an Ingress Controller and why do you need one?

### Answer

Ingress resource alone does nothing.

Controller implements ingress rules.

Examples:
- NGINX Ingress
- Traefik
- HAProxy
- Kong

### Architecture

```mermaid
flowchart LR
    User --> NGINXIngress
    NGINXIngress --> Service
    Service --> Pods
```

### Example

```bash
kubectl get ingress
kubectl get pods -n ingress-nginx
```

---

## 77. What are Network Policies and how do you restrict traffic?

### Answer

Network Policies act as Kubernetes firewalls.

Default:
- Allow all traffic

Network Policy:
- Allow specific traffic only

### Architecture

```mermaid
flowchart LR
    Frontend --> Backend
    Frontend -.blocked.-> Database
```

### Example

```yaml
kind: NetworkPolicy
```

---

## 78. How does kube-proxy manipulate iptables or IPVS to route traffic?

### Answer

kube-proxy watches Services and Endpoints.

It creates:
- iptables rules
- IPVS rules

to forward traffic.

### Architecture

```mermaid
flowchart LR
    ServiceIP --> kube-proxy
    kube-proxy --> Pod1
    kube-proxy --> Pod2
    kube-proxy --> Pod3
```

### Commands

```bash
iptables -t nat -L
ipvsadm -Ln
```

### Traffic Flow

```mermaid
sequenceDiagram
    Client->>ServiceIP: Request
    ServiceIP->>kube-proxy: Route
    kube-proxy->>Pod: Forward
```

---

# Quick Interview Summary

| Object | Purpose |
|----------|---------|
| ClusterIP | Internal access |
| NodePort | External node access |
| LoadBalancer | Cloud LB |
| Headless Service | Stateful apps |
| ExternalName | External DNS |
| CoreDNS | Service discovery |
| Ingress | HTTP routing |
| Ingress Controller | Implements Ingress |
| Network Policy | Firewall |
| kube-proxy | Traffic routing |
