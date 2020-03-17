# Folding@Home Client


## Kubernetes deployment

Manifests to deploy Folding@Home as daemonset in a Kubernetes cluster are available in the [".deploy/kubernetes"](./deploy/kubernetes) directory.

### CPU only

This **cpu-only** directory contains manifests to run Folding@Home using all available CPUs.
The Folding Power is set to `Medium` and Symetric Mult Processing (SMP) are enabled by default.

```bash
kubectl apply -f deploy/kubernetes/cpu-only
```