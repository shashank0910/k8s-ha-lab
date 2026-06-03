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
results/worker-failure-events.txt

### Experiment 3: Control Plane Failure

(To be completed)

## Results

Results and screenshots are stored in the repository.

