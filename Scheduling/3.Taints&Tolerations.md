## Taints and Tolerations

Taints are applied to nodes to repel certain pods, while tolerations are applied to pods to allow them to schedule on tainted nodes.

1. Adding a taint to a Node:

```bash
kubectl taint nodes node1 key=value:NoSchedule
```

2. Adding a toleration to a Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"    
  containers:
   - name: my-container
     image: my-image
```

### List taints on all nodes

```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
```

### List tolerations on all pods

```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,TOLERATIONS:.spec.tolerations
```


### Remove a taint from a node

```bash
kubectl taint nodes node1 key:NoSchedule-
```
