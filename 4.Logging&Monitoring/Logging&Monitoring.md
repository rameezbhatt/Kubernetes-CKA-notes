# Monitoring and Logging in Kubernetes

## Monitoring with Metrics Server

The Metrics Server is a cluster-wide aggregator of resource usage data in Kubernetes. It collects CPU and memory usage for nodes and pods, providing a foundation for basic monitoring and autoscaling decisions.

### Key Features:

1. Lightweight, memory-efficient implementation
2. Scales to support clusters with 5000 nodes
3. Resource metrics API support
4. No persistent storage; real-time metrics only

### Installation:

To install Metrics Server, use the following command:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Usage Examples:

1. View node metrics:
   ```bash
   kubectl top node
   ```

2. View pod metrics:
   ```bash
   kubectl top pod
   ```

3. View metrics for a specific namespace:
   ```bash
   kubectl top pod -n kube-system
   ```

4. Sort pods by CPU usage:
   ```bash
   kubectl top pod --sort-by=cpu
   ```

5. Display metrics in a specific format:
   ```bash
   kubectl top pod -o yaml
   ```

## Logging in Kubernetes

Kubernetes doesn't provide a native logging solution, but it offers ways to retrieve and manage application logs.

### Commands to View Application Logs:

1. View logs for a specific pod:
   ```bash
   kubectl logs my-pod
   ```

2. View logs for a specific container in a multi-container pod:
   ```bash
   kubectl logs my-pod -c my-container
   ```

3. Stream logs in real-time:
   ```bash
   kubectl logs -f my-pod
   ```

4. View logs from all pods with a specific label:
   ```bash
   kubectl logs -l app=my-label
   ```

5. View logs since a specific time:
   ```bash
   kubectl logs --since=1h my-pod
   ```

6. View the last N lines of logs:
   ```bash
   kubectl logs --tail=100 my-pod
   ```

7. View logs from previous container instances:
   ```bash
   kubectl logs -p my-pod
   ```

8. Output logs to a file:
   ```bash
   kubectl logs my-pod > pod_logs.txt
   ```

