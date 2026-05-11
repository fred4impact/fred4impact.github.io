---
title: "Strategy Two: Blue Green Deployments"
date: 2026-05-08T22:50:13+01:00
draft: true
tags: ["Argo-rollout","Blue-Green","Minikube", "ArgoCD"]
summary: "A Practical Argo Rollouts Walkthrough"
---
# Introduction
This section is the continuation of the `Deployment Strategies Demystified` were i covered the canary rolout stretagy of deployment and in other to better elaborate on the topic, i decided to split these into two section. here we will  be looking at the `Blue-Green Strategy`.


# The Core Idea

`Blue-green` takes a different approach. Rather than gradually shifting traffic, you spin up a complete parallel environment with the new version fully provisioned, fully warmed up before any user ever touches it. You test it privately via a preview service, and when you are ready, you flip the switch. 

The `transition` is instant and `atomic`. Not 20%, not 50% ` zero to one hundred` in a single operation.
The old environment `(blue)` stays running for a configurable window after the switch, giving you a fast rollback target if something surfaces in the first few minutes after go-live.

## Example Manifest 
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: nginx-bluegreen
  namespace: nginx-bluegreen
spec:
  replicas: 4
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
    blueGreen:
      activeService: nginx-active      # 100% of production traffic
      previewService: nginx-preview    # new version — only you can see it

      autoPromotionEnabled: false      # always promote manually
      scaleDownDelaySeconds: 300       # keep blue alive for 5 min after promotion
``` 

## The Deployment Flow
Apply the manifests and set up two port-forwards in separate terminals:

```bash 
# Terminal A — production traffic (what your users see)
kubectl port-forward svc/nginx-active 8080:80 -n nginx-bluegreen

# Terminal B — preview (only visible to you, for testing)
kubectl port-forward svc/nginx-preview 8081:80 -n nginx-bluegreen
```

## Now Trigger the Rollout 

```bash
kubectl argo rollouts set image nginx-bluegreen nginx=nginx:1.25 -n nginx-bluegreen
```

At this point something important happens: `http://localhost:8080 `continues serving nginx:1.24 with zero interruption to users. 

Meanwhile `http://localhost:8081` is now serving nginx:1.25  a complete, fully running environment that only you are hitting. This is your window to run smoke tests, integration checks, or any manual verification you need.

## When you are satisfied, flip the switch:
```bash 
kubectl argo rollouts promote nginx-bluegreen -n nginx-bluegreen
```

The `nginx-active service` selector flips to the new ReplicaSet instantly. All users move to `nginx:1.25` in one operation. The old blue environment stays alive for five minutes  your rollback window  then scales down.
When to Use `Blue-Green`

Reach for `blue-green` when you need a clean cutover with no mixed-version traffic. It is the right tool when your application has a breaking database schema change (old and new code cannot run simultaneously), when compliance or audit requirements demand a clean switch with a clear `before or after`, or when you want to run a full integration test suite against the new environment before any real user touches it.

### Canary vs Blue-Green: The Decision at a Glance
- Traffic during rollout: for `canary` it Split between versions while `Blue-green` is 100% on one version at a time
- Rollback speed: is `Instant `(abort) for canary while Instant (within scaleDownDelay) for blue-green
- Resource cost is Low (partial pods)for canary, while  High (2x pods during rollout) for blue-green
- Mixed-version traffic is Yes both versions serve users using canary while No only `one version `is active in blue-green
- DB schema compatibility Must be backward compatible, while in blue-green strategy Can be a breaking change
- Canary is Best for Catching subtle `regressions` at scale while Hard cutovers, compliance, pre-release testing in blue-green

The simplest heuristic: if you are asking "can I test this on real traffic gradually?" think `canary`. If you are asking "can I prepare and verify the new version before anyone sees it?" think `blue-green`.

## The Dashboard
One thing worth mentioning that makes Argo Rollouts genuinely enjoyable to use is the local dashboard. Run this in a separate terminal while you are experimenting:

# Run the following 

```bash 
kubectl argo rollouts dashboard -n nginx-demo
```
Then open `http://localhost:3100.` You get a live visual of both `ReplicaSets`, the current step, `traffic weights`, and `pod health` all updating in real time. It makes the abstract concept of traffic splitting very concrete, especially when you are learning.

## What I Took Away From This
Before building these demos my mental model of deployment was binary  old version or new version. Argo Rollouts changed how I think about it. A deployment is not a moment, it is a process with observable stages and decision points.

## A few things that stuck with me:
The initial deploy does not trigger canary steps. The canary steps only fire from the second deployment onwards. The first kubectl apply goes straight to 100% as the first stable revision. This trips up almost everyone the first time.

abort looks scarier than it is. When you abort a rollout the status briefly shows Degraded. It is not broken — it is mid-rollback. Give it ten seconds and it returns to Healthy on the previous stable revision.
Blue-green doubles your pod count during a rollout. With replicas: 4 you will temporarily have eight pods running.

 On `Minikube` with constrained resources this is worth keeping in mind. On production clusters you need to account for this in your node capacity planning. `scaleDownDelaySeconds` is your rollback window. In `blue-green`, once you promote, the clock starts on how long the old environment stays alive. Five minutes feels like a lot  until something goes wrong at minute four and you are glad it is still there.

## Next Steps
This walkthrough covered the fundamentals using manual promotion. The natural next step is wiring Argo Rollouts to your metrics connecting it to Prometheus so it can automatically abort a rollout if the error rate exceeds a threshold, without any human in the loop. That is where progressive delivery gets really powerful.
If you want to go further, have a look at:

- `Argo Rollouts Analysis and Experiments` — metric-driven automatic promotion/abort
- `Argo Rollouts with Ingress NGINX` — real traffic weight splitting via ingress annotations
- `Combining Argo Rollouts with ArgoCD` — GitOps-driven progressive delivery across multiple environments

All the manifests and scripts from this walkthrough are available in my GitHub repository.

If you found this useful or have questions, feel free to reach out. Happy deploying safely.