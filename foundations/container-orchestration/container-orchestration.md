---
title: "Container Orchestration"
description: "Learn Kubernetes — pods, deployments, services, namespaces, kubectl, and writing manifests."
sidebar_position: 1
---

# Container Orchestration

Running a single container is straightforward. Running hundreds of containers across multiple servers, handling failures, scaling up and down, and managing networking between them — that requires an orchestration platform. Kubernetes is the industry standard.

## What Is Container Orchestration?

Container orchestration automates the deployment, scaling, and management of containerized applications. Kubernetes (often abbreviated K8s) handles scheduling containers across nodes, restarting failed containers, load balancing traffic, and rolling out updates with zero downtime.

## Why It Matters

Kubernetes is the backbone of modern cloud-native infrastructure. Cloud providers offer managed Kubernetes services (EKS, GKE, AKS), and most large-scale deployments run on it. Understanding Kubernetes is essential for any infrastructure, DevOps, or SRE career path.

## What You'll Learn

- Kubernetes architecture: control plane and worker nodes
- Pods, Deployments, and ReplicaSets
- Services and networking within a cluster
- Namespaces for resource isolation
- Writing and applying YAML manifests
- Using `kubectl` to manage workloads
- ConfigMaps and Secrets

## Prerequisites

Complete [Containers](/learn/foundations/containers/containers/) and [Networking Fundamentals](/learn/foundations/networking-fundamentals/networking-fundamentals/) first.

## Next Step

Continue to [Infrastructure as Code](/learn/foundations/iac/iac/) to learn Terraform and how to define and provision infrastructure programmatically.
