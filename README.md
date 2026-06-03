# Kubernetes HA Validation Lab

## Overview

This project demonstrates a highly available Kubernetes cluster deployed using Kind on Ubuntu 24.04.

### Cluster Architecture

* 3 Control Plane Nodes
* 2 Worker Nodes
* External HAProxy Load Balancer
* Container Runtime: containerd
* Kubernetes Version: v1.33.1

## Objectives

* Validate Kubernetes High Availability
* Verify etcd quorum behavior
* Test worker node failure recovery
* Test control plane node failure recovery
* Validate pod rescheduling during failures

## Environment

| Component      | Value        |
| -------------- | ------------ |
| Host OS        | Ubuntu 24.04 |
| Kubernetes     | v1.33.1      |
| Kind           | v0.30.0      |
| Docker         | 29.x         |
| Control Planes | 3            |
| Workers        | 2            |

## Experiments

Experiment 1: Cluster Validation

* Verify cluster health
* Validate node registration
* Deploy sample workload
* Verify pod distribution

Experiment 2 – Worker Node Failure and Self-Healing
Objective

Validate Kubernetes self-healing capability by simulating a worker node failure and observing pod rescheduling behavior.

Test Procedure
Deployed nginx-ha application with 20 replicas.
Verified pod distribution across worker nodes.
Simulated worker node failure using:
docker stop prod-ha-worker
Monitored pod scheduling using:
kubectl get pods -o wide -w
Observations
Worker node transitioned to NotReady state.
Pods running on the failed worker entered Terminating state.
Kubernetes scheduler automatically rescheduled affected workloads.
Application availability was maintained during the failure.
Result

Kubernetes successfully detected node failure and rescheduled workloads without manual intervention, demonstrating self-healing and high-availability capabilities.

Evidence
screenshots/02-worker-failure.png
<img width="1205" height="760" alt="screenshots-02-worker-failure" src="https://github.com/user-attachments/assets/1a924f64-d510-42ff-8f04-3483d54b9e52" />
<img width="1045" height="343" alt="screenshots-03-worker-recovery" src="https://github.com/user-attachments/assets/6aa246e6-5752-409a-90a9-bd288cde7a59" />

results/worker-failure-events.txt

Experiment 3: Control Plane Failure and High Availability Validation

#### Objective

Validate Kubernetes control plane high availability by simulating a control plane node failure and verifying cluster functionality during the outage.

#### Test Procedure

1. Verified all three control plane nodes were in Ready state.

```bash
kubectl get nodes
```

2. Simulated control plane failure by stopping one control plane container.

```bash
docker stop prod-ha-control-plane2
```

3. Verified cluster accessibility.

```bash
kubectl get nodes
kubectl get pods -A
kubectl cluster-info
```

4. Verified etcd quorum remained intact.

```bash
kubectl get pods -n kube-system | grep etcd
```

5. Restored the failed control plane node.

```bash
docker start prod-ha-control-plane2
```

6. Confirmed successful node recovery.

```bash
kubectl get nodes
```

#### Observations

* The failed control plane node transitioned to NotReady state.
* Kubernetes API remained accessible throughout the failure.
* Workloads continued to operate normally.
* Remaining control plane nodes maintained cluster operations.
* etcd quorum was preserved with 2 of 3 members available.
* The failed control plane rejoined the cluster successfully after recovery.

#### Result

The Kubernetes cluster successfully tolerated a control plane node failure without service disruption, demonstrating control plane high availability and etcd quorum resilience.

#### Evidence

* screenshots/05-control-plane-failure.png
  <img width="892" height="818" alt="screenshots-04-master-failure" src="https://github.com/user-attachments/assets/713710ce-6432-4019-89c1-8338fc57f820" />
  <img width="725" height="61" alt="screenshots-05-etcd-quorum" src="https://github.com/user-attachments/assets/3593d669-6e1d-446a-9686-1b54c1ea510a" />
* screenshots/06-control-plane-recovery.png
  <img width="867" height="812" alt="Screenshot 2026-06-03 155950" src="https://github.com/user-attachments/assets/95d8f60e-358a-49ca-9f34-628349186b95" />
* results/control-plane-failure.txt
* results/etcd-status.txt

#### Key Learning

A multi-master Kubernetes architecture eliminates the control plane as a single point of failure and enables continuous cluster operations during node outages or maintenance activities.

