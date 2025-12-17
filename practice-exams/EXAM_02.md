# CKA Practice Exam 2

**Duration**: 2 hours  
**Total Tasks**: 18  
**Passing Score**: 74% (66 points minimum)  
**Exam Format**: Performance-based (hands-on tasks)

---

## Task 1: Multi-Container Pod (12 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
You need to create a pod with multiple containers working together - a main application container with a sidecar for log processing.

**Task**:  
Create a pod named `app-sidecar` in the `default` namespace with:
- Main container: `nginx:1.21` on port 80
- Sidecar container: `busybox:1.35` that tails logs from shared volume
- Init container: Waits for main container to be ready
- Shared volume: `emptyDir` mounted at `/shared` in all containers

**Verification Steps**:
```bash
# Verify pod is running
kubectl get pod app-sidecar
# Verify all containers are running
kubectl describe pod app-sidecar | grep -A 10 Containers
# Check init container completed
kubectl get pod app-sidecar -o jsonpath='{.status.initContainerStatuses[*].state}'
```

---

## Task 2: Deployment with Health Probes (10 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
A deployment needs comprehensive health checking to ensure pods are ready before receiving traffic and can recover from failures.

**Task**:  
Create a deployment named `web-deployment` in the `default` namespace with:
- 3 replicas
- Image: `nginx:1.21`
- Readiness probe: HTTP GET on `/` every 10 seconds (initial delay 5s)
- Liveness probe: HTTP GET on `/` every 20 seconds (initial delay 10s)
- Startup probe: HTTP GET on `/` with 30 failure threshold (period 10s)

**Verification Steps**:
```bash
# Verify deployment created
kubectl get deployment web-deployment
# Verify pods are running
kubectl get pods -l app=web-deployment
# Check probe configuration
kubectl describe deployment web-deployment | grep -A 15 Liveness
```

---

## Task 3: Service with Session Affinity (8 minutes)
**Domain**: Services & Networking (20% weight)

**Context**:  
An application requires session affinity to maintain user sessions on the same backend pod.

**Task**:  
Create a ClusterIP service named `sticky-service` in the `default` namespace that:
- Selects pods with label `app=web`
- Uses session affinity (ClientIP)
- Session timeout: 3 hours (10800 seconds)
- Exposes port 80

**Verification Steps**:
```bash
# Verify service created
kubectl get svc sticky-service
# Verify session affinity configured
kubectl describe svc sticky-service | grep -i affinity
# Verify endpoints
kubectl get endpoints sticky-service
```

---

## Task 4: ConfigMap with envFrom (10 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
An application needs multiple configuration values loaded as environment variables from a ConfigMap.

**Task**:  
1. Create ConfigMap `app-settings` in the `default` namespace with keys:
   - `ENV=production`
   - `DEBUG=false`
   - `TIMEOUT=30`

2. Create a pod named `config-pod` that uses `envFrom` to load all ConfigMap keys as environment variables

**Verification Steps**:
```bash
# Verify ConfigMap created
kubectl get configmap app-settings
# Verify pod is running
kubectl get pod config-pod
# Check environment variables
kubectl exec config-pod -- env | grep -E "ENV|DEBUG|TIMEOUT"
```

---

## Task 5: Secret from File (10 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
Sensitive credentials stored in files need to be loaded into a pod as a secret volume.

**Task**:  
1. Create files: `username.txt` (content: `admin`) and `password.txt` (content: `secret123`)

2. Create a secret `file-secret` in the `default` namespace from these files

3. Create a pod named `secret-pod` that mounts this secret as a volume at `/etc/secrets`

**Verification Steps**:
```bash
# Verify secret created
kubectl get secret file-secret
# Verify pod is running
kubectl get pod secret-pod
# Check secret files mounted
kubectl exec secret-pod -- ls -la /etc/secrets
kubectl exec secret-pod -- cat /etc/secrets/username.txt
```

---

## Task 6: StorageClass and Dynamic Provisioning (12 minutes)
**Domain**: Storage (10% weight)

**Context**:  
An application needs persistent storage that should only be provisioned when a pod is scheduled.

**Task**:  
1. Create StorageClass `fast-ssd` with:
   - Provisioner: `kubernetes.io/no-provisioner`
   - Volume binding mode: `WaitForFirstConsumer`
   - Reclaim policy: `Delete`

2. Create a PVC `dynamic-pvc` in the `default` namespace using this StorageClass requesting 10Gi

3. Create a pod named `storage-pod` that uses this PVC

**Verification Steps**:
```bash
# Verify StorageClass created
kubectl get storageclass fast-ssd
# Verify PVC is pending (waiting for consumer)
kubectl get pvc dynamic-pvc
# After pod creation, verify PVC bound
kubectl get pvc dynamic-pvc
# Verify pod is running
kubectl get pod storage-pod
```

---

## Task 7: Deployment Rollout Strategy (10 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
A deployment needs a controlled rolling update with specific surge and availability constraints.

**Task**:  
Update deployment `web-deployment` in the `default` namespace to use:
- Rolling update strategy
- Max surge: 2 pods
- Max unavailable: 1 pod
- Update image to `nginx:1.22`
- Monitor the rollout

**Verification Steps**:
```bash
# Verify rollout strategy
kubectl get deployment web-deployment -o yaml | grep -A 5 strategy
# Monitor rollout
kubectl rollout status deployment/web-deployment
# Verify new image
kubectl get deployment web-deployment -o jsonpath='{.spec.template.spec.containers[0].image}'
# Verify pods updated
kubectl get pods -l app=web-deployment
```

---

## Task 8: ClusterRole and ClusterRoleBinding (12 minutes)
**Domain**: Cluster Architecture (25% weight)

**Context**:  
A monitoring service account needs cluster-wide read permissions for nodes and namespaces.

**Task**:  
1. Create ClusterRole `node-viewer` that allows:
   - `get`, `list` on nodes
   - `get`, `list` on namespaces

2. Create ClusterRoleBinding `node-viewer-binding` that binds this role to ServiceAccount `cluster-monitor` in namespace `monitoring`
   - Note: Create the namespace and ServiceAccount if they don't exist

3. Verify permissions work correctly

**Verification Steps**:
```bash
# Verify ClusterRole created
kubectl get clusterrole node-viewer
# Verify ClusterRoleBinding created
kubectl get clusterrolebinding node-viewer-binding
# Test permissions
kubectl auth can-i get nodes --as=system:serviceaccount:monitoring:cluster-monitor
kubectl auth can-i list namespaces --as=system:serviceaccount:monitoring:cluster-monitor
```

---

## Task 9: NetworkPolicy with IP Block (15 minutes)

Create NetworkPolicy `external-api-policy` that:
- Applies to pods with label `app=api`
- Allows ingress from IP range `10.0.0.0/8` on port 8080
- Allows ingress from pods with label `app=frontend` on port 8080
- Denies all other ingress

---

## Task 10: Ingress with TLS (12 minutes)

Create an Ingress `secure-ingress` that:
- Uses ingress class `nginx`
- Host: `secure.example.com`
- TLS secret: `tls-secret`
- Routes to service `web-service` on port 80

Note: You don't need to create the TLS secret, just reference it.

---

## Task 11: StatefulSet Update Strategy (12 minutes)

Create StatefulSet `app-statefulset` with:
- 5 replicas
- Image: `nginx:1.21`
- Update strategy: RollingUpdate with partition 3
- Update image to `nginx:1.22` and verify only pods 0, 1, 2 update
- Then update remaining pods

---

## Task 12: Job with Parallelism (10 minutes)

Create a Job `parallel-processor` that:
- Completes 10 times
- Runs 3 pods in parallel
- Uses image `busybox:1.35`
- Command: `echo "Processing item $JOB_COMPLETION_INDEX" && sleep 10`
- Uses indexed completion mode

---

## Task 13: CronJob with Concurrency (10 minutes)

Create CronJob `hourly-sync` that:
- Runs every hour
- Concurrency policy: `Forbid` (don't run if previous job still running)
- Uses image `busybox:1.35`
- Command: `echo "Sync completed" && date`
- Keeps 5 successful jobs

---

## Task 14: DaemonSet with Node Selector (10 minutes)

Create DaemonSet `compute-monitor` that:
- Only runs on nodes with label `node-type=compute`
- Uses image `busybox:1.35`
- Command: `sh -c 'echo "Monitoring" && sleep 3600'`
- Has appropriate tolerations

---

## Task 15: LimitRange (10 minutes)

Create LimitRange `compute-limits` in namespace `production` that:
- Default CPU request: 100m, limit: 200m
- Default memory request: 128Mi, limit: 256Mi
- Max CPU per container: 2 cores
- Max memory per container: 4Gi
- Min CPU per container: 50m
- Min memory per container: 64Mi

---

## Task 16: Pod Disruption Budget (10 minutes)

Create PodDisruptionBudget `web-pdb` that:
- Applies to pods with label `app=web`
- Ensures minimum 3 pods available
- Works with deployment `web-deployment` (5 replicas)

---

## Task 17: Troubleshooting Deployment (12 minutes)
**Domain**: Troubleshooting (30% weight)

**Context**:  
A deployment update has been initiated but the rollout is stuck and not progressing. You need to diagnose and fix the issue.

**Scenario**:  
A deployment `api-deployment` in the `default` namespace is stuck and not updating to the new image version.

**Task**:  
1. Check rollout status and identify the issue
2. Check ReplicaSet status to see old vs new
3. Check pod status and events for errors
4. Identify the root cause (readiness probe, image pull, resource limits, etc.)
5. Fix the issue and verify rollout completes

**Verification Steps**:
```bash
# Verify deployment status
kubectl rollout status deployment/api-deployment
# Check ReplicaSets
kubectl get rs -l app=api
# Check pod events
kubectl get events --sort-by=.lastTimestamp
# Verify rollout completed
kubectl get deployment api-deployment -o jsonpath='{.status.conditions[*].status}'
```

---

## Task 18: Node Labeling and Taints (10 minutes)

1. Label node `worker-1` with `zone=east` and `node-type=compute`

2. Taint the node with:
   - Key: `dedicated`
   - Value: `special`
   - Effect: `NoSchedule`

3. Create a pod with matching toleration that can schedule on this node

---

## Scoring

- **Task 1**: 7 points
- **Task 2**: 6 points
- **Task 3**: 5 points
- **Task 4**: 6 points
- **Task 5**: 6 points
- **Task 6**: 7 points
- **Task 7**: 6 points
- **Task 8**: 7 points
- **Task 9**: 9 points
- **Task 10**: 7 points
- **Task 11**: 7 points
- **Task 12**: 6 points
- **Task 13**: 6 points
- **Task 14**: 6 points
- **Task 15**: 6 points
- **Task 16**: 6 points
- **Task 17**: 7 points
- **Task 18**: 6 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

