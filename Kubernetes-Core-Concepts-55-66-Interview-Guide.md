# Kubernetes Core Concepts (55-66) Interview Guide

## 55. Pod
```mermaid
flowchart TB
Pod-->App
Pod-->Sidecar
```
Pod is the smallest deployable unit in Kubernetes.

## 56. Pod Lifecycle
```mermaid
stateDiagram-v2
Pending-->Running
Running-->Succeeded
Running-->Failed
```

## 57. Sidecar Use Case
```mermaid
flowchart LR
App-->Fluentd-->Elasticsearch
```

## 58. Labels and Selectors
```mermaid
flowchart LR
Service-->Selector-->Pods
```

## 59. Labels vs Annotations
Labels are used for selection. Annotations store metadata.

## 60. ReplicaSet
```mermaid
flowchart TB
ReplicaSet-->Pod1
ReplicaSet-->Pod2
ReplicaSet-->Pod3
```

## 61. Deployment
```mermaid
flowchart TB
Deployment-->ReplicaSet-->Pods
```

## 62. Rolling Update
```mermaid
flowchart LR
v1-->v1_v2-->v2
```

## 63. Rollback
```mermaid
flowchart LR
v1-->v2
v2-->Rollback
Rollback-->v1
```

## 64. StatefulSet
```mermaid
flowchart LR
StatefulSet-->mysql0
StatefulSet-->mysql1
StatefulSet-->mysql2
```

## 65. DaemonSet
```mermaid
flowchart LR
DaemonSet-->Node1
DaemonSet-->Node2
DaemonSet-->Node3
```

## 66. Job vs CronJob
```mermaid
flowchart LR
CronJob-->Job-->Pod
```
