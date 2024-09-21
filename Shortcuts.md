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
These commands can simplify resource creation and YAML generation during the exam.