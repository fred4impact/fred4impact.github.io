---
title: "Deployment Strategies Demystified"
date: 2026-05-08T15:59:51+01:00
draft: true
tags: ["Argo-rollout","canary","Minikube", "ArgoCD"]
summary: "A Practical Argo Rollouts Walkthrough"
---

A hands-on guide to canary and blue-green deployments no cloud account required.

# Introduction

If you have ever deployed an `application` and held your breath waiting to see if it breaks production, this post is for you.

For a long time, deployment meant one thing: stop the `old` version, start the `new` one, and hope for the best. Maybe you had a `rollback` plan. Maybe you didn't. Either way, the moment you ran kubectl apply, every single user was immediately on the new version  bugs and all.

There is a better way, and it is called `progressive delivery`.
In this post I want to walk you through how I explored two of the most powerful `deployment strategies`  canary and blue-green. using `Argo Rollouts` running entirely on my local machine with Minikube. No cloud account. No AWS bill. Just a laptop, a terminal, and a simple nginx app to make the concepts concrete.

By the end you will understand not just how each `strategy` works, but when to reach for one over the other  which is the question that actually matters in practice.

# What Is Argo Rollouts?

`Argo Rollouts` is a `Kubernetes controller` that replaces the standard Deployment resource with a Rollout resource. The key difference is that a standard Deployment replaces all your pods at once (or in a rolling wave with no traffic control). A `Rollout` gives you fine-grained control over how traffic shifts between the `old and new versio` of your app.

It ships with two primary strategies out of the box:

- `Canary`: gradually shift a percentage of traffic to the new version
- `Blue-Green`: run two full environments in parallel, then switch instantly

The controller also integrates with ingress controllers, service meshes, and metric providers so you can make `promotion decisions` based on real `data — error rates, latency, custom business metrics`. But for this walkthrough we are keeping it simple and doing everything manually so the mechanics are clear.

## Setting Up the Local Environment
Everything in this demo runs on Minikube.
If you have not already got it installed, grab it from the official Minikube docs.

## Start Minikube

I will be using macbook pro for these demo, and too start minikube on mac make sure you have `docker dekstop` installed on yours and make sure its running 

## First open a  terminal on your mac and run the below

```bash
minikube start --cpus=2 --memory=3072 --driver=docker
```
## Secondly,  Install the Argo Rollouts Controller
The controller runs inside your cluster and manages all Rollout resources.

```bash
kubectl create namespace argo-rollouts

kubectl apply -n argo-rollouts \
  -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml

# Wait  while to check the status 
kubectl rollout status deployment/argo-rollouts -n argo-rollouts
```

## Install the kubectl Plugin
This gives you the kubectl argo rollouts subcommand and the local dashboard.

```bash
curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64
chmod +x kubectl-argo-rollouts-linux-amd64
sudo mv kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts

# Confirm it works
kubectl argo rollouts version
```

## Strategy One: Canary Deployments

### The Core Idea
A `canary deployment` shifts a small percentage of live traffic to the new version first. You observe it `error` `rates`, `latency`, user feedback  and only `promote` to 100% once you are confident. If something looks wrong, you abort and traffic instantly returns to the stable version. The name comes from the old `mining practice `of sending a canary into a coal mine before the miners the canary catches the problem first.

### The Rollout Manifest
The key difference from a standard Deployment is the strategy. canary block. Instead of Kubernetes deciding how to roll out, you define explicit steps.

### Sample Rollout Yaml manifest 

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: nginx-rollout
  namespace: nginx-demo
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.24
          ports:
            - containerPort: 80

  strategy:
    canary:
      stableService: nginx-stable
      canaryService: nginx-canary

      steps:
        - setWeight: 20        # 20% of traffic goes to the new version
        - pause:
            duration: 2m       # wait 2 minutes automatically
        - setWeight: 50        # increase to 50%
        - pause: {}            # hold here indefinitely — wait for manual promotion
```
`Two services` are required `nginx-stable` and `nginx-canary`. Argo Rollouts manages their `selectors automatically`, injecting a unique pod template hash to keep traffic separated between the old and new ReplicaSets. 

You do not touch the services  the controller does it for you.
Triggering and Watching a Rollout Once the initial deployment is healthy, trigger a rollout by updating the image:

### Deploy the nginx-rollout.yaml 
```bash
kubectl argo rollouts set image nginx-rollout nginx=nginx:1.25 -n nginx-demo
```
Then watch what happens in real time:

```bash
kubectl argo rollouts get rollout nginx-rollout -n nginx-demo --watch
```

You will see the `rollout progress` through each step, pausing at the `pause`: {} on step 4. At that `point 50%` of traffic is on `nginx:1.25 and 50% ` is still on `nginx:1.24`. This is the moment where in a real system you would be checking your error rate dashboards and latency graphs.
When you are happy, promote it:

###  Promote Now and watch
 
```bash
kubectl argo rollouts promote nginx-rollout -n nginx-demo
```

### Try To simulate a bad deploy and see rollback in action:

```bash 
kubectl argo rollouts set image nginx-rollout nginx=nginx:broken-image -n nginx-demo

# Abort thW Rollout and watch 
kubectl argo rollouts abort nginx-rollout -n nginx-demo
# Traffic immediately returns 100% to nginx:1.25
```

## When to Use Canary

Reach for canary when you want to validate with real traffic before committing. It is the right tool when your database schema is backward compatible (both versions can talk to the same DB simultaneously), when you have enough traffic to get meaningful signal from a subset of users, and when you want to catch subtle performance regressions that only show up under real load.

## Watch out
 ![Strategy-Two:-Blue-Green-Deployments](http://wwww.example.com)