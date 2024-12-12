# API Objects

## Objectives
- Learn how to manage Kubernetes API objects using `kubectl`.
- Understand `kubectl` commands for viewing, creating, and editing objects.
- Explore `kubectl` dry-run and diff features for manifest validation and comparison.

## Steps

### Context Management
1. **Get the current context:**
   ```bash
   kubectl config get-contexts
   ```
   Example output:
   ```
   CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
   *         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
   ```

2. **Change context if necessary:**
   ```bash
   kubectl config use-context kubernetes-admin@kubernetes
   ```

---

### API Resources
3. **List available API resources:**
   ```bash
   kubectl api-resources | more
   ```

4. **Explain specific resources:**
   ```bash
   kubectl explain pods | more
   ```

---

### Creating and Managing Pods
5. **Create a pod using a YAML file:**
   Save the following content as `pod.yaml`:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: hello-world
   spec:
     containers:
     - name: hello-world
       image: ghcr.io/hungtran84/hello-app:1.0
   ```
   Apply the manifest:
   ```bash
   kubectl apply -f pod.yaml
   ```

6. **Inspect `pod.spec` and `pod.spec.containers`:**
   ```bash
   kubectl explain pod.spec | more
   kubectl explain pod.spec.containers | more
   ```

7. **List running pods:**
   ```bash
   kubectl get pod
   ```

8. **Delete the pod:**
   ```bash
   kubectl delete pod hello-world
   ```

---

### Validating with `kubectl dry-run`
9. **Server-side validation:**
   ```bash
   kubectl apply -f deployment.yaml --dry-run=server
   ```

10. **Client-side validation:**
    ```bash
    kubectl apply -f deployment.yaml --dry-run=client
    ```

11. **Validate an incorrect manifest:**
    Example error:
    ```bash
    kubectl apply -f deployment-error.yaml --dry-run=server
    Error from server (BadRequest): error when creating "deployment-error.yaml": Deployment in version "v1" cannot be handled as a Deployment: strict decoding error: unknown field "spec.replica"
    ```

---

### Generating YAML Manifests
12. **Generate YAML for a deployment:**
    ```bash
    kubectl create deployment nginx --image=nginx --dry-run=client -o yaml | more
    ```

13. **Generate YAML for a pod:**
    ```bash
    kubectl run pod nginx-pod --image=nginx --dry-run=client -o yaml | more
    ```

14. **Save generated YAML to a file:**
    ```bash
    kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deployment-generated.yaml
    ```

15. **Apply the generated manifest:**
    ```bash
    kubectl apply -f deployment-generated.yaml
    ```

16. **Clean up:**
    ```bash
    kubectl delete -f deployment-generated.yaml
    ```

---

### Comparing Manifests with `kubectl diff`
17. **Apply a deployment:**
    ```bash
    kubectl apply -f deployment.yaml
    ```

18. **Diff the deployment with a modified manifest:**
    ```bash
    kubectl diff -f deployment-new.yaml | more
    ```

---

## Summary
- Used `kubectl` to explore API resources and manage objects.
- Created and deleted a pod using YAML manifests.
- Leveraged `kubectl dry-run` for both server-side and client-side validations.
- Generated YAML manifests for reuse and validation.
- Compared existing deployments with modified manifests using `kubectl diff`.
