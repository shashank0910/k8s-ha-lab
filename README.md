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

### Experiment 1: Cluster Validation

* Verify cluster health
* Validate node registration
* Deploy sample workload
* Verify pod distribution

### Experiment 2: Worker Node Failure

(To be completed)

### Experiment 3: Control Plane Failure

(To be completed)

## Results

Results and screenshots are stored in the repository.

