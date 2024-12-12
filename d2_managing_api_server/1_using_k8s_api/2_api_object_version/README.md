# Lab: API Object Discovery

## Exploring API Groups
Let's ask the API Server for the API Groups it knows about.

```bash
kubectl api-resources | more
```

### Sample Output:
```plaintext
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
...
services                          svc          v1                                     true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
...
```

---

## Listing Objects in a Specific API Group
To list the objects available in a specific API Group, use the `--api-group` flag:

```bash
kubectl api-resources --api-group=apps
```

### Sample Output:
```plaintext
NAME                  SHORTNAMES   APIVERSION   NAMESPACED   KIND
controllerrevisions                apps/v1      true         ControllerRevision
daemonsets            ds           apps/v1      true         DaemonSet
deployments           deploy       apps/v1      true         Deployment
replicasets           rs           apps/v1      true         ReplicaSet
statefulsets          sts          apps/v1      true         StatefulSet
```

---

## Using `kubectl explain`
You can use `kubectl explain` to dig deeper into a specific API Object.

### Example:
```bash
kubectl explain deployment | head
```

### Key Information:
- The **KIND** and **VERSION** indicate the API Group in the format `group/version`.
- For example, `extensions/v1beta1` may appear, but note that this is deprecated.

---

## Viewing Supported API Versions
You can retrieve the supported API versions on the API Server in the form `group/version`:

```bash
kubectl api-versions | sort | more
```

### Sample Output:
```plaintext
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2
batch/v1
certificates.k8s.io/v1
coordination.k8s.io/v1
discovery.k8s.io/v1
...
```

This command helps identify all the API Groups and their versions available in the cluster.
