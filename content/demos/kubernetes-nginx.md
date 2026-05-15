---
title: "Deploy Nginx on Kubernetes"
date: 2026-05-15
tags: ["kubernetes", "demo"]
---

# 🚀 Kubernetes Nginx Deployment

## 🧠 Goal
Deploy a simple Nginx app on a Kubernetes cluster.

---

## 📐 Architecture

```mermaid
graph TD
  User --> Ingress
  Ingress --> Service
  Service --> Pod