# Kubernetes Resource Creation Shortcuts

## Key Commands

1. Create Pod:
   ```
   kubectl run nginx --image=nginx
   ```

2. Generate Pod YAML:
   ```
   kubectl run nginx --image=nginx --dry-run=client -o yaml
   ```

3. Create Deployment:
   ```
   kubectl create deployment --image=nginx nginx
   ```

4. Generate Deployment YAML:
   ```
   kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
   ```

5. Save Deployment YAML:
   ```
   kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
   
   ```
6. Get all pods with label 'app=myapp' 

```
kubectl get pods -l app=myapp
```
7. Get all resources with label 'environment=production'

```
kubectl get all -l environment=production
```

8. Adding a taint to a Node:

 ```bash
kubectl taint nodes node1 key=value:NoSchedule
```

9. List taints on all nodes

```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
```

10. List tolerations on all pods

```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,TOLERATIONS:.spec.tolerations
```


11. Remove a taint from a node

```bash
kubectl taint nodes node1 key:NoSchedule-
```

12. Labeling a Node:

```bash
kubectl label nodes node1 type=large
```

13. List labels on all nodes

```bash
kubectl get nodes --show-labels
```

14. List node selectors on all pods

```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,NODE_SELECTOR:.spec.nodeSelector
```

15. Remove a label from a node

```bash
kubectl label nodes node1 type-
```

