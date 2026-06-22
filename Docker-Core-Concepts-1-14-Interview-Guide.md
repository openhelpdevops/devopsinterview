# Docker Core Concepts (1-14) Interview Guide

This guide contains concise answers and Mermaid diagrams for all 14 Docker core concepts.

## 1. Docker vs VM
```mermaid
flowchart LR
Host-->DockerEngine-->Containers
```
Docker shares the host OS; VMs run separate guest OSes.

## 2. Docker Architecture
```mermaid
flowchart LR
Client-->Daemon
Daemon-->Containers
Daemon<-->Registry
```

## 3. Image vs Container
```mermaid
flowchart LR
Image-->Container
```
Image = template, Container = running instance.

## 4. Container Lifecycle
```mermaid
stateDiagram-v2
Created-->Running
Running-->Stopped
Stopped-->Deleted
```

## 5. docker run Internals
```mermaid
sequenceDiagram
User->>Daemon: docker run
Daemon->>Registry: pull image
Daemon->>Host: create container
```

## 6. Namespaces
```mermaid
flowchart TB
Container-->PID
Container-->NET
Container-->MNT
```
Isolation mechanism.

## 7. cgroups
```mermaid
flowchart LR
Container-->CPU
Container-->Memory
```
Resource limits.

## 8. Dockerfile
```mermaid
flowchart LR
Dockerfile-->Build-->Image
```

## 9. Layers and Cache
```mermaid
flowchart TB
Base-->RUN-->COPY-->CMD
```

## 10. CMD vs ENTRYPOINT
```mermaid
flowchart LR
ENTRYPOINT-->Process
CMD-->Process
```

## 11. COPY vs ADD
```mermaid
flowchart LR
File-->COPY
File-->ADD
URL-->ADD
```

## 12. docker exec
```mermaid
flowchart LR
Host-->docker_exec-->Container
```

## 13. CPU and Memory Limits
```mermaid
flowchart LR
Container-->1CPU
Container-->512MB
```

## 14. Container Logs
```mermaid
flowchart LR
Container-->Logs-->Terminal
```
