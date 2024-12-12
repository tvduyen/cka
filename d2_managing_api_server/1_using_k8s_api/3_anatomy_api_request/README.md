# Lab: Anatomy of an API Request in Kubernetes

## Objectives

- Understand how to create a pod using YAML.
- Explore different verbosity levels in `kubectl` commands to inspect API requests and responses.
- Learn how to use `kubectl proxy` to interact with the Kubernetes API server directly.
- Observe resource watch behavior and manage resource states.
- Troubleshoot common issues such as authentication failures and missing resources.

---

## Steps

### Step 1: Create a Pod Using YAML
1. Apply a YAML manifest to create a pod:
   ```bash
   kubectl apply -f pod.yaml
   ```
   Expected output:
   ```
   pod/hello-world created
   ```

2. Verify the pod is running:
   ```bash
   kubectl get pod hello-world
   ```
   Example output:
   ```
   NAME          READY   STATUS    RESTARTS   AGE
   hello-world   1/1     Running   0          27s
   ```

---

### Step 2: Inspect API Requests with Verbosity Levels
1. Increase verbosity to inspect resource URL, `VERB`, `API Path`, and `Response` code:
   ```bash
   kubectl get pod hello-world -v 6
   ```

2. Add HTTP Request Headers with verbosity level 7:
   ```bash
   kubectl get pod hello-world -v 7
   ```

3. Add Response Headers and truncated Response Body with verbosity level 8:
   ```bash
   kubectl get pod hello-world -v 8
   ```

4. View the full API response metadata with verbosity level 9:
   ```bash
   kubectl get pod hello-world -v 9
   ```

---

### Step 3: Interact with the Kubernetes API Server Using `kubectl proxy`
1. Start a `kubectl proxy` session:
   ```bash
   kubectl proxy &
   ```

2. Use `curl` to fetch the pod resource details:
   ```bash
   curl http://localhost:8001/api/v1/namespaces/default/pods/hello-world | head -n 10
   ```

3. Terminate the proxy session:
   ```bash
   fg
   ctrl+c
   ```

---

### Step 4: Watch, Exec, and Log Requests
1. Start watching pod updates:
   ```bash
   kubectl get pods --watch -v 6 &
   ```

2. Delete the pod to observe updates:
   ```bash
   kubectl delete pods hello-world
   ```

3. Recreate the pod:
   ```bash
   kubectl apply -f pod.yaml
   ```

4. Stop the watch session:
   ```bash
   fg
   ctrl+c
   ```

5. Access pod logs:
   ```bash
   kubectl logs hello-world
   kubectl logs hello-world -v 6
   ```

6. Access logs directly via the API server:
   ```bash
   kubectl proxy &
   curl http://localhost:8001/api/v1/namespaces/default/pods/hello-world/log
   ```

7. Stop the proxy session:
   ```bash
   fg
   ctrl+c
   ```

---

### Step 5: Authentication Failure Demonstration
1. Backup the kubeconfig:
   ```bash
   cp ~/.kube/config ~/.kube/config.ORIG
   ```

2. Modify the username in the kubeconfig:
   ```bash
   vi ~/.kube/config
   ```
   Change `kubernetes-admin` to `kubernetes-admin1`.

3. Attempt to access the cluster (expect a 403 Forbidden response):
   ```bash
   kubectl get pods -v 6
   ```

4. Restore the original kubeconfig:
   ```bash
   cp ~/.kube/config.ORIG ~/.kube/config
   ```

5. Verify cluster access:
   ```bash
   kubectl get pods
   ```

---

### Step 6: Handle Missing Resources
1. Query a non-existent pod:
   ```bash
   kubectl get pods nginx-pod -v 6
   ```

---

### Step 7: Create and Delete a Deployment
1. Apply a deployment YAML:
   ```bash
   kubectl apply -f deployment.yaml -v 6
   ```

2. List deployments:
   ```bash
   kubectl get deployment
   ```

3. Clean up resources:
   ```bash
   kubectl delete deployment hello-world -v 6
   kubectl delete pod hello-world
   ```

---

## Summary

In this lab, you learned to interact with the Kubernetes API server using `kubectl` commands with varying verbosity levels. You explored how to inspect request details, access logs, and troubleshoot issues like authentication errors and missing resources. Additionally, you observed resource updates and understood how to create and delete Kubernetes resources effectively.
